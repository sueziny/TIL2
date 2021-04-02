# TIL2

package stream.data.test;

import java.io.DataOutputStream;
import java.io.FileOutputStream;
import java.io.IOException;

/*
 * 간단한 성적들을 파일로 출력하는 코드를 작성
 * 만약에 score.dat 파일 형식이 지원되지 않아서 출력되는 에러 메세지는 무시...
 * 바이트 계열 스트림은 출력되는 데이터 형식이 문자 기반이 아닌 Binary Data(이진 데이터)이기에
 * 문서편집기 보아도 알 수 없는 형식으로 열린다.
 * 그래서 결국 이를 확인하기 위해서는 
 * score.dat파일에 출력된 Binary Data를 다시 DataInputStream을 통해서 자바 기본형(int)으로 자동 변환시켜서 읽어들여야 한다.
 *  
 */

public class DataOutputStreamTest1  {

	public static void main(String[] args) throws IOException{
		
		int[ ] scores = {100,90,70,75,66};//문자 기반이 아니다...바이트계열 출력 스트림 사용...(FileOutStream)
		String outFile = "src\\scores.dat";
		//1.
		DataOutputStream dos = new DataOutputStream(new FileOutputStream(outFile));
		
		System.out.println("score.dat파일이 생성되었습니다.");
		
		//2. 배열 안에 들어있는 점수를 생성된 파일로 출력...writeInt()
		for(int score : scores) dos.writeInt(score);
		
		
	}

}
----------------------------------------------------------------------------------------------

package stream.data.test;

import java.io.DataInputStream;
import java.io.EOFException;
import java.io.FileInputStream;
import java.io.IOException;

public class DataOutputStreamTest2 {

	public static void main(String[] args) {
		DataInputStream dis = null;

		try {
			dis = new DataInputStream(new FileInputStream("src\\scores.dat"));
			int data = 0;

			while (true) {
				data = dis.readInt();
				System.out.println(data);
			}
		} catch (EOFException e) {

		} catch (IOException e) {

		} finally {
			try {
				dis.close();
			} catch (IOException e) {

			}
		}
	}
}

---------------------------------------------------------------------

package stream.data.test;

import java.io.DataInputStream;
import java.io.EOFException;
import java.io.FileInputStream;
import java.io.IOException;

public class DataOutputStreamTest3 {

	public static void main(String[] args) {
		DataInputStream dis = null;

		try {
			dis = new DataInputStream(new FileInputStream("src\\scores.dat"));
			int data = 0;

			while ((data = dis.readInt()) != -1) {
				//data = dis.readInt();
				System.out.println(data);
			}
		} catch (EOFException e) {

		} catch (IOException e) {

		} finally {
			try {
				dis.close();
			} catch (IOException e) {

			}
		}
	}
}

---------------------------------------------------------

package stream.data.test;

import java.io.DataInputStream;
/*
 * scores.dat파일의 내용을 readInt()로 읽어들여서
 * 각각의 점수를 출력하고
 * 모든 점수의 촐합을 구하는 로직을 작성..
 * EOFException은 컴파일 시점에 잡혀지지 않고
 * 실행지점에서 잡혀지기 때문에
 * main메소드 선언부에서 throws로 던져질 수 없다.
 * --------------------------------------
 * 
 * main메소드 선언부에서 throws로 예외를 전져서
 * main을 호출한 곳(JVM)에서 예외를 처리할 수 있는 것은
 * Compile Exception계열만이 가능했다.
 */
import java.io.EOFException;
import java.io.FileInputStream;
import java.io.IOException;

public class DataOutputStreamTest4 {

	public static void main(String[] args) {
	
		int sum =0;
		int score = 0;
		
		DataInputStream dis = null;
		
		//1. 스트림 생성
		
		try {
			//1. 스트림 생성
			dis = new DataInputStream(new FileInputStream("src\\scores.dat"));
			System.out.println("Stream Creating");
			
			while(true) {
				score = dis.readInt();
				System.out.println(score);
				sum += score;
			}
		} catch (EOFException e) {
			System.out.println("모든 점수의 총합은 "+sum+"점!!");
		} catch (IOException e) {

		} finally {
			try {
				dis.close();
			} catch (IOException e) {

			}
		}
		
	}
}

