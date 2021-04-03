# TIL2

package chatting.step3.Client;
/*
 * 키보드로 데이터를 읽어들여서 
 * 서버로 보낸다.
 * -------------------------
 * 다시 서버가 보낸 메세지를 읽어서
 * 클라이언트 자신의 콘솔창에 메세지를 출력
 * ::
 * 입력 --->
 * 출력 --->
 */

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.Socket;

public class ChattClient {

	// 필드 선언
	BufferedReader br;
	PrintWriter pw;
	Socket s;

	// Stream
	public void net() {
		try {
			s = new Socket("127.0.0.1", 60000);
			System.out.println("Socket Creating...");

			br = new BufferedReader(new InputStreamReader(System.in));
			pw = new PrintWriter(s.getOutputStream(), true);
			//

			// 서버가 보낸 메세지만 읽어들이는 작업을 전담하는 쓰레드를 생성
			ClientThread ct = new ClientThread(s);

			System.out.println("Client Stream Creating...");

			while (true) {
				String line = br.readLine();// 키보드로 입력 받은 데이터 읽어서
				pw.println(line);// 서버로 보낸다
			}
		} catch (Exception e) {
			System.out.println("서버와의 연결이 끊어졌습니다.");
		} finally {
			try {
				br.close();
				pw.close();

			} catch (IOException e) {

			}
		}
	}

	public static void main(String[] args) {
		ChattClient cc = new ChattClient();
		cc.net();
	}
}

class ClientThread extends Thread {
	Socket s;
	BufferedReader br1;

	ClientThread(Socket s) {
		this.s = s;

	}

	public void run() {
		try {
			br1 = new BufferedReader(new InputStreamReader(s.getInputStream()));
			while (true) {
				String str = br1.readLine();
				System.out.println("서버 메세지 " + str);
			}
		} catch (IOException e) {

		}
	}
}
--------------------------------------------------------------------------------

package chatting.step3.server;

/*
 * 여러명의 클라이언트의 접속을 받아서
 * 문자 기반의 메세지를 주고 받을 수 있는
 * 서버 측 로직을 작성
 * ::
 * ChattServer ----Process
 * 			|-----> ServerSocket, ArrayList, Socket
 * 					broadcast()
 * 
 * ChattThread ----Thread
 * 
 * 			|-----> Socket, BufferedReader, PrintWriter
 */
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.ArrayList;

public class ChattServer {
	// Process의 필드를 지정
	ServerSocket server;
	Socket s;
	ArrayList<Chatthread> list;

	public ChattServer() {
		list = new ArrayList<Chatthread>();
	}

	public void broadcast(String message) {
		for (Chatthread t : list) {
			t.sendMessage(message);
		}
	}

	public void net() {
		try {
			server = new ServerSocket(60000);
			System.out.println("Server Ready...");

			while (true) {// 윙윙윙...waiting for...
				s = server.accept();
				Chatthread ct = new Chatthread(s, this);
				list.add(ct);
				ct.start();// 작업이 병렬적으로 진행되는 focus!!
			}
		} catch (Exception e) {

		} finally {

		}
	}

	public static void main(String[] args) {
		ChattServer process = new ChattServer();
		process.net();
	}
}

//서버와 연결된 클라이언트와 메세지를 주고 받는 일만 전담하는 쓰레드...작업쓰레드
class Chatthread extends Thread {
	Socket s;
	BufferedReader br;
	PrintWriter pw;
	ChattServer chattServer;

	public Chatthread(Socket s, ChattServer chattServer) {
		this.s = s;
		this.chattServer = chattServer;

		System.out.println(s.getInetAddress() + "님이 접속하셨습니다.");
		chattServer.broadcast(s.getInetAddress() + "님이 채팅방에 들어오셨습니다.");
	}

	public void sendMessage(String str) {
		pw.println(str);
	}

	public void run() {
		try {
			br = new BufferedReader(new InputStreamReader(s.getInputStream()));
			pw = new PrintWriter(s.getOutputStream(), true);

			while (true) {
				String line = br.readLine();
				chattServer.broadcast(line);
			}
		} catch (IOException e) {
			// 연결이 끊어지면 로직이 이곳으로..
			System.out.println(s.getInetAddress() + "님이 접속을 끊으셨습니다.");
			chattServer.broadcast(s.getInetAddress() + "님이 채팅방을 나가셨습니다.");
			//
			chattServer.list.remove(this);
		}
	}
}
