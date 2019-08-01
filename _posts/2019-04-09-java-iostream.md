---
title: "Java IO 입출력"
layout: post
category: Java
tags: [Java]
excerpt: "Java의 IO 입출력에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

자바 공부를 하면서 참고한 서적입니다.

_명품자바에센셜(생능출판사, 황기태), 이것이 자바다(한빛미디어, 신용권), 디자인패턴과 리팩토링_

## Stream

  자바에서 데이터는 [스트림(Stream)](https://baekjungho.github.io/java-inputoutput/)을 통해 입출력된다. 스트림에는 입력스트림(InputStream)과 출력스트림(OutputStream)이 있습니다.
  스트림 클래스는 크게 두 종류로 Byte기반과 Character기반으로 나뉩니다. 바이트 기반 스트림은 그림, 문자, 멀티미디어 등 모든 종류의
  데이터를 받고 보낼 수 있고, 문자 기반 스트림은 오직 문자만 받고 보낼 수 있습니다.

  __입력 : 외부파일 --> 프로그램으로 출력__

  __출력 : 프로그램 --> 외부파일로 출력__

  __입출력 스트림을 사용할때는 예외처리를 꼭 해주어야 합니다.__

  입출력 스트림을 사용할때는 거의 while문을 많이사용합니다.

  > InputStream : 프로그램에 데이터가 들어오는 것
  >
  > OutputStream : 프로그램에서 데이터가 나가는 것

  - __java.io의 주요 클래스__

    |클래스명  |특징|
    |-----------|----|
    |File|  파일 시스템의 파일 정보를 얻기 위한 클래스|
    |Console|  콘솔로부터 문자를 입출력하기 위한 클래스|
    |InputStream / OutputStream|  바이트 단위 입출력을 하기 위한 최상위 입출력 클래스|
    |FileInputStream/FileOutputStream|  바이트 단위 입출력을 하기 위한 하위 입출력 클래스|
    |DataInputStream/DataOutputStream|  바이트 단위 입출력을 하기 위한 하위 입출력 클래스|
    |ObjectInputStream/ObjectOutputStream|  바이트 단위 입출력을 하기 위한 하위 입출력 클래스|
    |PrintStream|  바이트 단위 입출력을 하기 위한 하위 입출력 클래스|
    |Reader / Writer|  문자 단위 입출력을 위한 최상위 입출력 클래스|
    |FileReader/FileWriter|  문자 단위 입출력을 위한 하위 입출력 클래스|
    |InputStreamReader/OutputStreamWriter|  문자 단위 입출력을 위한 하위 입출력 클래스|
    |PrintWriter|  문자 단위 입출력을 위한 하위 입출력 클래스|
    |BufferedReader/BufferedWriter|  문자 단위 입출력을 위한 하위 입출력 클래스|

  -----------------------------------------------------------------------------

### InputStream

  - __InputStream Method__

    |메소드명  |특징|
    |-----------|----|
    |read()|  입력 스트림으로부터 1바이트를 읽고 읽은 바이트를 리턴(int)|
    |read(byte[] b)|  입력 스트림으로부터 읽은 바이트들을 매개값 b에 저장하고 실제로 읽은 바이트 수 리턴(int)|
    |read(byte[] b, int off, int len)|  입력 스트림으로부터 len개의 바이트만큼 읽고 매개값으로 주어진 바이트 배열 b[off]부터 len개 까지 저장. 실제로 읽은 바이트 수인 len개 리턴(int), 만약 len개를 모두 읽지 못하면 실제로 읽은 바이트 수 리턴|
    |close()|  입력 스트림을 닫는다|

  한글 바이트 수 판별법은 ANSI(EUC-KR)로 되어있을경우는 한글 한 글자당 2byte, UTF-8로 되어있을경우
  한글 한 글자당 3byte로 따집니다.

  - __read()__

    read() 메소드는 입력 스트림으로부터 읽을 바이트 수가 없다면 -1(음수값)을 리턴합니다.
    처음 부터 마지막까지 바이트를 읽는 방법은 다음과 같습니다.

    ```java
    InputStream is = new FileInputStream("C:\programfiles\java\stream.java");
    int readByte;
    while((readByte=is.read()) != -1) {
      // 실행코드
    }
    is.close();
    ```

  - __read(byte[] b)__

    매개값으로 주어진 배열의 길이만큼 루핑을 합니다. 많은 양의 바이트를 읽을때는
    이 메소드가 좋습니다.

    ```java
    InputStream is = new FileInputStream("C:\programfiles\java\stream.java");
    byte[] readBytes = new byte[100];
    int readByteNo;
    while((readByteNo=is.read(readBytes)) != -1) {
      // 실행코드
    }
    is.close();
    ```

  다음과 같은 방식이 주로 사용하는 입력 스트림 사용 방식 입니다. test.txt파일에는
  hello world를 적어줬습니다.

  위에서 while문안에 readByteNo=is.read(readBytes)이런식으로 적어준것은 만약 while문 위에서
  int readByteNo = is.read(readBytes) 이렇게 할 경우 출력시 이상하게 출력될 수 있으므로
  __꼭 while문 안에서 작성해야 합니다.__

  ```java
  import java.io.FileInputStream;
  import java.io.InputStream;

  public class ReadExample {
	public static void main(String[] args) throws Exception {
		InputStream is = new FileInputStream("C:/temp/test.txt");
		int readByteNo;
		byte[] readBytes = new byte[3]; // 읽어드릴 배열의 크기
		String data = "";
		while(true) {
			// 읽은 바이트들을 배열 readBytes에 저장하고 실제로 읽은 바이트 수를 리턴
			// 더이상 읽을 바이트가 없으면 -1 리턴
			readByteNo = is.read(readBytes);
			System.out.println(readByteNo);
			if(readByteNo == -1) break;
			data += new String(readBytes, 0, readByteNo);
		}
		System.out.println(data);
		is.close();
	 }
  }
  ```

  다음은 read(byte[] b, int off, int len)를 사용한 예제를 보겠습니다.

  ```java
  import java.io.FileInputStream;
  import java.io.InputStream;

  public class ReadExample {
	public static void main(String[] args) throws Exception {
		InputStream is = new FileInputStream("C:/temp/test.txt");
		int readByteNo;
		byte[] readBytes = new byte[15];
		readByteNo = is.read(readBytes, 2, 8);
		for(int i=0; i<readBytes.length; i++) {
			System.out.print(readBytes[i]+" ");
		}
		is.close();
	 }
  }
  ```

  결과 : 0 0 104 101 108 108 111 32 119 111 0 0 0 0 0

  즉, 2번째 인덱스부터 8글자를 저장하고 나머지 빈 공간은 0으로 채웁니다.

  -----------------------------------------------------------------------------

### OutputStream

  - __OutputStream Method__

    |메소드명  |특징|
    |-----------|----|
    |write(int b)|  1바이트를 보낸다.|
    |write(byte[] b)|  바이트 배열b의 모든 바이트를 보낸다.|
    |write(byte[] b, int off, int len)|  바이트 배열 b[off]부터 len개까지의 바이트를 보낸다.|
    |flush()|  버터에 잔류하는 모든 바이트를 출력한다.|
    |close()|  출력 스트림을 닫는다|

    위 메소드들은 반환값이 없습니다.(void)

  - __write()__

    ```java
    import java.io.FileOutputStream;
    import java.io.OutputStream;

    public class WriteExample {
	  public static void main(String[] args) throws Exception {
		OutputStream os = new FileOutputStream("C:/temp/test.txt");
		byte[] data = "Hello".getBytes();
		for(int i=0; i<data.length; i++) {
			os.write(data[i]);
		}
		  os.flush();
		  os.close();
  	 }
    }
    ```

    getBytes()로 Hello를 data에 저장하고 배열의 길이만큼 반복해서 os.write()로
    H, e, l, l, o를 하나씩 파일에 출력하는 것입니다.

    __즉, 출력 = 프로그램에서 나가서 파일이나 다른곳에 값을 변환 시키는 것__

    위 프로그램을 실행하고 다시 InputStream으로 프로그램을 돌리면 원래 저장되어있던
    Hello World가 Hello로 바뀐 것을 알 수 있습니다.

  - __write(byte[] b)__

    다음과 같은 방식으로 한꺼번에 바이트를 출력할 수 있습니다.

    ```java
    import java.io.FileOutputStream;
    import java.io.OutputStream;

    public class WriteExample2 {
	  public static void main(String[] args) throws Exception {
		OutputStream os = new FileOutputStream("C:/temp/test.txt");
		byte[] data = "Hello Java".getBytes();
		os.write(data);
		os.flush();
		os.close();
	   }
    }
    ```

  다음은 write(byte[] b, int off, int len)를 사용한 예제를 보겠습니다.
  바이트를 잘라서 출력한다는 의미와 비슷합니다.

  ```java
  import java.io.FileOutputStream;
  import java.io.OutputStream;

  public class WriteExample3 {
	public static void main(String[] args) throws Exception {
		OutputStream os = new FileOutputStream("C:/temp/test.txt");
		byte[] data = "HELLO".getBytes();
		os.write(data, 1, 2); // EL 출력
		os.flush();
		os.close();
	 }
  }
  ```

  -----------------------------------------------------------------------------

### Reader

  Reader는 문자단위로 읽습니다.

  메소드로는 read(), read(char[] cbuf), read(char[] cbuf, int off, int len), close()가 있습니다.

  text에 안녕하세요를 적고 utf-8로 저장한 후 다음 예제를 실행해 보겠습니다.

  ```java
  import java.io.FileReader;
  import java.io.Reader;

  public class ReadExample1 {
	public static void main(String[] args) throws Exception {
		Reader reader = new FileReader("C:/temp/test.txt");
		int readData;
		while(true) {
			readData = reader.read();
			if(readData == -1) break;
			System.out.print((char)readData);
		}
		reader.close();
	 }
  }
  ```

  출력결과 : 공백 안녕하세요

  이런식으로 앞에 공백이 붙어서 출려하는데 그 이유는 `0xfeff` 라는 `Byte Order Mark`가
  붙어서 출력되어 utf-8은 한글 한 글자당 3바이트인데 앞 0xfeff가 포함되어
  18byte를 가지게 됩니다.
  0xfeff를 없애려면 메모장에 한글을 적고 저장하는 것이 아니라 프로램에서 출력으로
  저장되게 하면 안녕하세요만 저장되고 15byte를 가지게 됩니다.

  -----------------------------------------------------------------------------

### Writer

  Writer는 문자기반 출력 스트림입니다. write() 한 문자를 보내는 메소드이며, write(String str)은
  문자열을 전부 보내는 메소드입니다, write(char[] cbuf)는 주어진 문자배열 cbuf에 저장된 문자를 모두 보냅니다.

  아래 예제는 문자 배열에 문자를 저장하고 한꺼번에 출력하는 예제 입니다.

  ```java
  import java.io.FileWriter;
  import java.io.Writer;

  public class WriteExample2 {
	public static void main(String[] args) throws Exception {
		Writer writer = new FileWriter("C:/temp/test.txt");
		char[] data = "Hello Java".toCharArray();
		writer.write(data);
		writer.flush();
		writer.close();
	 }
  }
  ```

  다음 예제는 String 문자열을 한꺼번에 출력하는 예제 입니다.

  이 방식이 더 자주 사용되는 방식입니다.

  ```java
  import java.io.FileWriter;
  import java.io.Writer;

  public class WriteExample3 {
	public static void main(String[] args) throws Exception {
		Writer writer = new FileWriter("C:/temp/test.txt");
		String data = "Hello Java Programming";
		writer.write(data);
		writer.flush();
		writer.close();
	 }
  }
  ```

  -----------------------------------------------------------------------------

### Console

  콘솔(Console)은 시스템을 사용하기 위해 키보드로 입력을 받고 화면을 출력하는 소프트웨어입니다.

  1. System.in : 입력스트림
  2. System.out : 출력 스트림
  3. System.err : 에러 스트림

  > 키보드 입력 스트림 얻기 : InputStream is = System.in;
  >
  > 출력 스트림 얻기 : OutputStream os = System.out;

  -----------------------------------------------------------------------------

### File

  File 클래스는 파일 크기, 속성, 이름 등의 정보를 얻어 낼 수 있고, 파일 생성 및 삭제가 가능합니다.
  디렉토리도 만들 수 있습니다. 하지만 파일의 데이터를 읽고 쓰고는 불가능 합니다.

  - 파일 존재 여부

    ```java
    // 파일이 존재하면 true, 존재하지 않으면 false리턴
    boolean isExist = file.exists();
    ```

  - __File Method__

    |메소드명  |특징|리턴타입|
    |-----------|----|-------|
    |createNewFile()|  새로운 파일 생성| boolean|
    |mkdir()|  새로운 디렉터리 생성| boolean|
    |mkdirs()|  경로상 없느 몯든 디렉터리 생성| boolean|
    |delete()|  파일이나 디렉터리 삭제| boolean|
    |canExecute()|  실행할 수 있는 파일인지 여부| boolean|
    |canRead()|  읽을 수 있는 파일인지 여부| boolean|
    |canWrite()|  수정 및 저장할 수 있는 파일인지 여부| boolean|
    |getName()|  파일 이름을 리턴| String|
    |getParent()|  부모 디렉터리를 리턴| String|
    |getPath()|  전체 경로 리턴| String|
    |getParentFile()|  부모 디렉터리를 File 객체로 생성 후 리턴| File|
    |isDirectory()|  디렉터리 인지 여부| boolean|
    |isFile()|  파일인지 여부| boolean|
    |isHidden()|  숨김파일인지 여부| boolean|
    |lasModified()|  마지막 수정 날짜 및 시간을 리턴| long|
    |length()|  파일의 크기를 리턴| long|
    |list()|  디렉터리에 포함된 파일 및 서브디렉터리 목록 전부를 String 배열로 리턴| String[]|
    |list(FilenameFilter filter)| 디렉터리에 포함된 파일 및 서브디렉터리 목록 중 FilenameFilter에 맞는 것만 리턴 | String[]|
    |listFiles()|  디렉터리에 포함된 파일 및 서브디렉터리 목록 전부를 File 배열로 리턴| File[]|
    |listFiles(FilenameFilter filter)|  디렉터리에 포함된 파일 및 서브디렉터리 목록 중 FilenameFilter에 맞는 것만 리턴| File[]|

## Filter Stream

  보조스트림(Filter stream)은 다른 스트림과 연결되어 여러 가지 편리한 기능을 제공해주는 스트림을 말합니다.
  보조스트림은 문자 변환, 입출력 성능 향상, 객체 입출력 등의 기능을 제공합니다. 또한 보조스트림은 다른
  보조스트림에도 연결 될 수 있어서 `Stream Chain(스트림 체인)`을 만들 수 있습니다.

  - __Architecture__

    > 보조스트림 변수 = new 보조스트림(연결스트림)

  - __Stream Chain__

    보조스트림 InputStreamReader를 BufferedReader에 연결하는 방법

    ```java
    InputStream is = System.in;
    InputStreamReader reader = new InputStreamReader(is); // 보조스트림 : InputStreamReader
    BufferedReader br = new BufferedReader(reader); // 보조스트림 : BufferedReader
    ```

  -----------------------------------------------------------------------------

### InputStreamReader & OutputStreamWriter

  InputStreamReader는 바이트 입력 스트림에 연결되어 문자 입력 스트림인 Reader로 변환시키는 보조스트림입니다.

  - __FileInputStream을 Reader로 변환__

    ```java
    FileInputStream fis = new FileInputStream("파일경로");
    Reader reader = new InputStreamReader(fis);
    ```

  - __FileOutputStream을 Reader로 변환__

    ```java
    FileOutputStream fos = new FileOutputStream("파일경로");
    Writer writer = new OutputStreamReader(fos);
    ```

  -----------------------------------------------------------------------------

### BufferedInputStream & BufferedReader

  [대용량 입출력에 대한 이해](https://baekjungho.github.io/java-inputoutput/)

  BufferedInputStream & BufferedReader는 `버퍼를 제공하는 보조 스트림` 입니다. 내부 버퍼에 데이터를 읽고 저장해둡니다.

  바이트 입력 스트림에 연결되어 사용됩니다.

  그리고 읽어올때는 프로그램 외부가 아닌 버퍼로부터 읽어서 읽기 성능이 향상 됩니다.

  > 외부 파일 --> 버퍼 --> 프로그램 출력

  BufferedInputStream은 최대 8192바이트, BufferedReader는 최대 8192문자를 저장할 수 있습니다.

  > 버퍼에 데이터를 채우고 버퍼로부터 읽기
  >
  > BufferedInputStream bis = BufferedInputStream(바이트입력스트림);
  >
  > BufferedReader br = BufferedReader(문자입력스트림);

  -----------------------------------------------------------------------------

### BufferedOutputStream & BufferedWriter

  BufferedOutputStream & BufferedWriter는 `버퍼를 제공하는 보조 스트림` 입니다. 프로그램에서 전송한 데이터를 내부 버퍼에 쌓아두고
  버퍼가 꽉차면 버퍼의 모든 데이터를 한꺼번에 보냅니다.

  > 프로그램 --> 버퍼 --> 외부파일 출력

  바이트 출력 스트림에 연결되어 사용됩니다.

  > BufferedOutputStream bos = BufferedOutputStream(바이트출력스트림);
  >
  > BufferedWriter bw = BufferedWriter(문자출력스트림);

  -----------------------------------------------------------------------------

### DataInputStream & DataOutputStream

  DataInputStream & DataOutputStream은 기본 타입 입출력 보조 스트림 입니다.

  위 스트림을 사용할때 조심해야할 점은 출력순서가 int - char - double이면
  입력 순서도 int - char - double이어야 합니다.

  ```java
  FileOutputStream fos = new FileOutputStream("파일경로");
  DataOutputStream dos = new DataOutputStream(fos);

  FileInputStream fis = new FileInputStream("파일경로");
  DataInputStream dis = new DataInputStream(fis);
  ```

  -----------------------------------------------------------------------------

### ObjectInputStream & ObjectOutputStream

  ObjectInputStream & ObjectOutputStream은 객체를 입출력 할 수 있습니다.

  > ObjectInputStream : 객체 역 직렬화
  >
  > ObjectOutputStream : 객체 직렬화

  ```java
  FileOutputStream fos = new FileOutputStream("파일경로");
  ObjectOutputStream oos = new ObjectOutputStream(fos);

  FileInputStream fis = new FileInputStream("파일경로");
  ObjectInputStream ois = new ObjectInputStream(fis);
  ```

  -----------------------------------------------------------------------------

### Format String

  [형식화된 문자열(Format String)](https://baekjungho.github.io/java-inputoutput/)은 C언어와 사용법이 비슷합니다.

  > printf(형식화된 문자열, 매개값)

  1. %d : decimal(정수)
  2. %f : float(실수)
  3. %s : string(문자열)

  > % [매개값 순번] [flags] [전체자릿수] [소수자릿수] 변환문자(Conversion)

  %와 Conversion은 필수이며, [ ]는 선택사항입니다. flags는 -, 0으로 쓰이는데
  -는 오른쪽으로 공백이 채워지고, 0은 왼쪽에 0으로 공백을 채웁니다.
  예를들어 %-8d는 8자리 정수를 나타내며, 오른쪽 빈자리는 공백으로 둡니다.
  %08d는 8자리 정수를 나타내며, 왼쪽 빈자리를 0으로 채웁니다.(00012345)
  %10.2f는 소수점 이상 7자리 소수점 이하 2자리를 나타내며 왼쪽 빈자리는 공백으로 둡니다.

  > System.out.printf("%,d원\n", 1234) --> 1,234원
  >
  > System.out.format("%c", b[i]); System.out.format("%02X ", b[i]);

  위 두 가지의 특징은 줄바꿈을 하지 않습니다.

  System.out.format은 문자나 16진수등의 형태로 출력할 수 있습니다.

  > [Reference](https://library1008.tistory.com/5)

  -----------------------------------------------------------------------------

### PrintStream & PrintWriter

  PrintStream & PrintWriter는 printf(), print(), println() 메소드를 가지고 있는
  보조 스트림 입니다. 저희가 자주사용했던 `System.out이 PrintStream 타입`이기 때문에
  위 메소드를 사용할 수 있었습니다.

  PrintStream & PrintWriter는 보조스트림이기 때문에 역시 `바이트 스트림`과 연결되어 사용 됩니다.

  ```java
  FileOutputStream fos = new FileOutputStream("파일경로");
  PrintSream ps = new PrintSream(fos);

  ps.println("Hello World");
  ```

  위 프로그램을 사용하면 메모장에 Hello World라고 출력된걸 볼 수 있습니다.
