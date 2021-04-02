# TIL2

package stream.console.test;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

/*
 * 키보드로 읽어들인 데이터를 콘솔로 출력하는 로직을 작성(System.out.println())하는 로직을 작성
 * ::
 * 소스코드 패턴
 * 1. 스트림 객체 생성
 * 2. 읽어들인다.
 * 3. 콘솔로 출력한다.
 * 
 */
public class KeyboardInputTest1 {

	public static void main(String[] args) {
		// 1. 입력 스트림만 필요..2개(기본(node), 보조(filter))
		System.out.println("1. 스트림 생성...데이터를 키보드로 입력하세요>>");
		InputStreamReader ir = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(ir);

		// 2. 생성한 스트림의 기능을 사용... 읽어들인다.
		System.out.println("2. 데이터를 읽어들인다...");

		try {
			String line = br.readLine();
			while (line != null) {
				// 3. 읽어들인 데이터를 콘솔로 출력한다.
				System.out.println("Reading Date :: " + line);
				line = br.readLine();// 이 부분이 더 추가되어야 계속 읽어서 출력한다.
			}
		} catch (IOException e) {
			e.printStackTrace();
		} finally {
			try {
				//ir.close(); <-- 굳이 쓸 필요 없음
				br.close();
			} catch (IOException e) {

			}
		}

	}

}
----------------------------------------------------------------------------------

package stream.console.test;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

/*
 * 1. 스트림 객체를 생성....입력, 출력
 * 2. while문 안에서 계속 읽어들이고 
 * 3. 읽은 데이터를 출력
 * ::
 * 키보드로 입력한 데이터를 
 * 콘솔로 출력하는 로직을 작성
 * ::
 * 입력 --2개 - InputStreamReader, BufferedReader
 * 출력 --X - System.out.println()
 */

public class KeyboardInputTest2 {

	public static void main(String[] args) throws IOException {
		// 1.
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		System.out.println("InputStream Creating....키보드로 데이터 입력..\n");

		// 2.
		String line = null;
		try {
			while ((line = br.readLine()) != null) {// 2
				if (line.equals("exit")) {
					System.out.println("시스템을 종료합니다...입력금지");
					break;
				}
				System.out.println("Reading Data ::" + line);// 3
			}
		} finally {
			br.close();
		}

	}

}
