# TIL2

package chatting.step1.client;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.Socket;
import java.net.UnknownHostException;

/*
 * Socket 통신에서 클라이언트 측 로직을 담고 있는 프로세스
 * 소켓을 생성해서 요청을 하고자하는 서버측으로 연결을 시도
 * 이때 서버측이 가지고 있는 포트와 동일한 포트를 이용해야 연결이 될 수 있다.
 * ::
 * 스트림 코드
 * 키보드로 읽어들인 데이터를 
 * 서버로 보내는 로직을 작성
 * 
 */
public class ChattClient {
	
	Socket s;
	BufferedReader br;//자제적으로 생성
	PrintWriter pw;// 소켓으로 리턴 받아서..
	
	public void net() {
		
		try {
			s = new Socket("127.0.0.1",5500);
			System.out.println("Socket Creating");
			
			br = new BufferedReader(new InputStreamReader(System.in));
			pw = new PrintWriter(s.getOutputStream(), true); //auto flushing
			System.out.println("Stream Creating...");
			
			String line = "";
			while((line = br.readLine())!=null) pw.println(line);
			
		} catch (UnknownHostException e) {
			System.out.println("서버와의 연결에 실패했습니다...");
		} catch (IOException e) {
			
		}
	
	}
	public static void main(String[] args) {
		ChattClient chattClient = new ChattClient();
		chattClient.net();
	}

}
--------------------------------------------------

package chatting.step1.server;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.ServerSocket;
import java.net.Socket;

/*
 * Socket 통신에서 서버측 로직을 담고 있는 프로세스
 * 클라이언트의 접속을 받아들이기 위해서
 * 1 ServerSocket을 생성
 * 2.accept()를 통해서 서버로 접속해오는 클라이언트를 받아서 클라이언트 소켓을 리턴한다.
 * ::
 * 클라이언트가 보낸 메세지 읽어서
 * 서버 자신의 콘솔창에 메세지를 출력하는 로직을 작성
 * 입력스트림---->소켓으로부터 받아서 만든다.
 * 
 */
public class ChattServer {
	ServerSocket server;
	Socket s;
	BufferedReader br;
	
	public void net() {
		
		try {
			server = new ServerSocket(5500);
			System.out.println(" Server Ready..");
			
			s=server.accept();
			System.out.println("Client's Socket...Returned");
			
			br = new BufferedReader(new InputStreamReader(s.getInputStream()));
			System.out.println("Stream Creating...Using Socket..");
			
			String line = null;
			while((line = br.readLine())!=null) System.out.println("Client가 보낸 메세지"+line);
			
		}catch(IOException e) {
			System.out.println("서버와의 연결이 끊어졌습니다.");
		}
		
	}
	
	public static void main(String[] args) {
		ChattServer chattServer = new ChattServer();
		chattServer.net();
		

	}

}

