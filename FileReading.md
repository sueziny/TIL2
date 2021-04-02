# TIL2

package stream.file.test;

import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.EOFException;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.OutputStream;

public class DataInputOutputTest5 {

	public static void main(String[] args) throws IOException{
	
		String inputFile = "src\\watermelon.png";
		String outputFile = "src\\water.png";
		OutputStream water = null;
		
		DataInputStream dis = null;
		DataOutputStream dos = null;
		try {
			dis = new DataInputStream(new FileInputStream(inputFile));
			dos = new DataOutputStream(new FileOutputStream(outputFile));
		
			int data = 0;
			while((data = dis.readInt())!=-1) dos.writeInt(data);
			
		}catch(EOFException e) {
			
		}finally {
			
		}
	}

}
--------------------------------------------------
package stream.file.test;

import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

/*
 * 파일에 입력되 내용을 읽어서 
 * 콘솔창으로 출력하는 로직을 작성
 * ::
 * 스트림 
 * 입력 2개--FileReader, BufferedReader
 * 
 */
public class FileReadingTest1 {

	public static void main(String[] args) throws IOException {
		//1. 스트림 생성 -- 입력 2개 (FileReader, BufferdReader)
		BufferedReader br = new BufferedReader(new FileReader("src\\hope.txt"));
		
		try {
			//2. while문에서 파일의 내용을 읽고
			String line = null;
			while((line=br.readLine())!=null) {
				//3. 콘솔로 출력하기...
				System.out.println(line);
			}
		}finally {
			br.close();
		}
	}
}
------------------------------------------------------------
package stream.file.test;

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

/*
 * 파일에 입력되 내용을 읽어서 
 * 또다른 파일로 출력하는 로직을 작성
 * ::
 * 스트림 
 * 입력 2개--FileReader, BufferedReader
 * 출력 2개 -- FileWriter, BufferedWriter
 * ::
 * 1. 스트림 생성..4개 생성
 * 2. 파일의 내용을 읽어서
 * 3. 또다른 파일로 출력(Sink)
 * 
 */
public class FileReadingAndWritingTest2 {

	public static void main(String[] args) throws IOException {
		//1. 스트림 생성 -- 입력 2개 (FileReader, BufferdReader)
		BufferedReader br = new BufferedReader(new FileReader("src\\hope.txt"));
		BufferedWriter bw = new BufferedWriter(new FileWriter("src\\result.txt"));
		try {
			//2. while문에서 파일의 내용을 읽고
			String line = null;
			while((line=br.readLine())!=null) {
				//3. 또다른 파일로 출력(Sink)...이부분이 변경
				bw.write(line);
				bw.newLine();
			}
			//bw.flush();//auto flushing 기능...데이터를 모아두지 말고 입력되는 족족 바로 출력한다.
		}finally {
			br.close();
			bw.close();
		}
	}
}
--------------------------------------

package stream.file.test;

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.io.PrintWriter;

/*
 * 파일에 입력되 내용을 읽어서 
 * 또다른 파일로 출력하는 로직을 작성
 * ::
 * 스트림 
 * 입력 2개--FileReader, BufferedReader
 * 출력 2개 -- FileWriter, BufferedWriter
 * ::
 * 1. 스트림 생성..4개 생성
 * 2. 파일의 내용을 읽어서
 * 3. 또다른 파일로 출력(Sink)
 * 
 */
public class FileReadingAndWritingTest3 {

	public static void main(String[] args) throws IOException {
		//1. 스트림 생성 -- 입력 2개 (FileReader, BufferdReader)
		BufferedReader br = new BufferedReader(new FileReader("src\\hope.txt"));
		PrintWriter pw = new PrintWriter(new FileWriter("src\\result.txt"),true);
		try {
			//2. while문에서 파일의 내용을 읽고
			String line = null;
			while((line=br.readLine())!=null) {
				//3. 또다른 파일로 출력(Sink)...이부분이 변경
				pw.println(line);
			}
			pw.flush();//auto flushing 기능...데이터를 모아두지 말고 입력되는 족족 바로 출력한다.
		}finally {
			//br.close();
			//pw.close();
		}
	}
}
------------------------------------------

package stream.file.test;

import java.io.BufferedReader;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;
import java.io.PrintWriter;

/*
 * hope.txt파일에 문자 데이터를 읽어서 --- FileReader계열 사용
 * 다른 경로의 result.txt 파일로 문자 데이터를 출력---FileWriter Character계열 사용
 * ::
 * 스트림 코드 작성 패턴
 *1. 입력, 출력 스트림을 한꺼번에 생성
 *
 *2. 반복문 안에서 데이터를 전부 다 읽어서 
 *
 *3.반복문 안에서 읽어들인 데이터를 Sink방향으로 전부 다 출력
 *	--> 문자 데이터를 출력할 때는 읽어들이는 족족 출력하지 않고 
 *		일정량 만큼 모아두었다가 한꺼번에 출력하는 패턴을 보인다.
 *		1) auto flush기능 -- flush()
 *		2) 자원을 닫을 때 그 안에 있는 데이터를 한꺼번에 출력--close()
 *
 */

class FileReadWriterReview {
	FileReadWriterReview() throws IOException  {
		// 로직을 여기에다가 작성...1--->2-->3
		
		BufferedReader br = null;
		PrintWriter pw = null;
		try {

			 br = new BufferedReader(new FileReader("src\\hope.txt"));
			 pw = new PrintWriter("C:\\data\\result.txt");//디렉토리가 존재해야지만 출력 스트림이 생성...그 안에 출력 파일도 만들어진다.
			System.out.println("1. Stream Creating....");
			String line = null;
			System.out.println("2. FileReading...and Printing..");
			while((line = br.readLine())!=null) pw.println(line);
		} finally {
			br.close();
			pw.close();
		}
	}
}

public class FileReadWriterReviewTest4 {

	public static void main(String[] args) throws IOException {
		
		new FileReadWriterReview();
		
		
	}

}




