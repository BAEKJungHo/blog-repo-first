---
title: "입출력의 모든것"
layout: post
category: Java
tags: [Java]
excerpt: "Java에서 입출려과 관련된 클래스와 사용법에 대해 배워봅시다."
author: BAEKJungHo
---

* content
{:toc}

자바 공부를 하면서 참고한 서적입니다.

_명품자바에센셜(생능출판사, 황기태), 이것이 자바다(한빛미디어, 신용권)_

## 입출력의 모든 것

  자바에서 입출력을 하기 위한 방법들을 심층적으로 알아봅시다.

  자바의 입출력은 [스트림(Stream)](https://baekjungho.github.io/java-iostream/)을 통해 실시됩니다.
  스트림(Stream)은 크게 `입력스트림 & 출력스트림`이 있습니다. 입력스트림에는 read()라는 메소드가 따라다니고
  출력스트림에는 write()메소드라는 것이 따라 다닙니다.

  스트림을 사용할때는 예외가 발생할 수 있으니 예외 처리를 해주어야 합니다. `try-catch사용 or
  throws Exception사용`

  > 입력스트림 : 프로그램이 데이터를 입력 받을때
  >
  > 출력스트림 : 프로그램이 데이터를 보낼때

  __즉, 프로그램을 기준으로 데이터가 들어오면 입력스트림, 나가면 출력스트림 입니다.__

  스트림 클래스는 크게 2종류로 나뉘는데 `바이트 스트림 & 문자 스트림`입니다.
  바이트 스트림(Byte Stream)은 문자, 그림, 멀티미디어 등 모든 종류의 데이터를 받고 보낼 수 있고,
  문자 스트림은(Character Stream)은 오직 문자를 받고 보낼 수 있게 특화 되어있습니다.

  바이트 스트림은 ~Stream으로 끝나고 문자스트림은 ~Reader, ~Writer로 끝납니다.

  바이트 스트림 중에서 입력스트림은 `~InputStream`으로 끝나고 출력스트림은 `~OutputStream`으로 끝납니다.
  또한, 문자 스트림에서 입력 문자스트림은 `~Reader`로 끝나고 문자 출력스트림은 `~Writer`로 끝납니다.

  다음은 스트림을 사용할때 while문이 꼭 따라오는데 다음 두 종류로 나눌 수 있습니다.
  공식같이 사용되는 코드이니 꼭 외우셔야 합니다. 첫 번째는 바이트 스트림에서
  `read()메소드`를 사용한 것이고 두번째는 문자 스트림인 BufferedReader에서 `readLine()`을 사용한 것입니다.

  - __read()__

    1바이트씩 읽고 4바이트 int 타입으로 리턴, 리턴된 4바이트 중 끝의 1바이트에만 데이터가 들어있고
    `바이트를 더이상 읽을 수 없으면 -1을 리턴` 따라서 리턴값을 받을 int형 변수가 필요합니다.

    ```java
    // byte배열을 사용하지 않은 경우
    InputStream is = new FileInputStream("C:\temp\test.txt");
    int readByteNo;
    // while((readByteNo = is.read()) > 0)
    while((readByteNo = is.read()) != -1) {
        ...
    }
    is.close(); // InputStream 닫기

    // byte배열을 사용하여 배열의 길이만큼 바이트를 읽고 배열에 저장, 읽은 바이트 수 리턴
    InputStream is = new FileInputStream("C:/temp/test.txt");
    byte[] readBytes = new byte[100]; // 100개씩 바이트를 읽을 수 있다.
    int readByteNo;
    while((readByteNo = is.read(readBytes)) != -1) {
        ...
    }
    is.close(); // InputStream 닫기
    ```

  - __readLine()__

    readLine()은 BufferedReader에서 사용하며 한 줄씩 읽습니다. 문자열을 읽기때문에
    String 클래스의 문자열 변수가 필요합니다.

    ```java
    FileReader fr = new FileReader("C:/temp/test.txt");
    BufferdReader br = new BufferedReader(fr);
    // BufferedReader br = new BufferedReader(new FileReader("C:/temp/text.txt"));
    String line = ""; // String line = null;
    while((line = br.readLine()) != null) {
        ...
    }
    ```

  다음은 출력스트림의 write()메소드에 대해서 알아봅시다. write()메소드는
  매개 값으로 주어진 배열의 모든 바이트를 출력스트림으로 보냅니다.
  [getBytes()](https://baekjungho.github.io/java-api)는 문자열을 바이트 배열로 변환합니다.

  ```java
  OutputStream os = new FileOutputStream("C:/temp/test.txt");
  String str = "Java";
  byte[] data = str.getBytes(); // data에 Java를 바이트 배열로 변환
  os.write() // OutputStream(출력스트림)으로 배열의 모든 바이트를 보낸다.
  os.flush(); // 버퍼에 잔류하는 데이터 모두 출력스트림으로 보내고 버퍼를 비운다.
  os.close(); // OutputStream 닫기
  ```

  출력스트림은 내부에 `작은 버퍼(buffer)`가 있어서 데이터가 출력되기 전에 버퍼에 쌓여있다가
  순서대로 출력됩니다. 따라서 flush()라는 메소드를 통해서 잔류하고 있는 데이터를
  모두 출력스트림으로 보내고 버퍼를 비워줘야 합니다. 그리고 마지막에는 close()로 사용한
  스트림을 닫아줍니다.

  > 입력스트림과 함께 하는 것 : read(), close()
  >
  > 출력스트림과 함께 하는 것 : write(), flush(), close()

  -----------------------------------------------------------------------------

  다음은 문자 입력 스트림(~Reader)의 read()메소드에 대해서 알아 봅시다.
  read()는 문자(2바이트)를 읽고 4바이트 int형으로 리턴합니다. 맨 끝 2바이트에만 문자 데이터가 들어있습니다.
  다음 공식처럼 사용되는 코드를 외웁시다.

  ```java
  // char형 배열을 사용하지 않은 경우
  File file = new File("C:/temp/test.txt");
  FileReader fr = new FileReader(file);
  int readData;
  while((readData = fr.read()) != -1) {
      ...
  }
  fr.close();

  // char형 배열을 사용하여 지정한 크기만큼씩 읽기
  FileReader fr = new FileReader("C:/temp/test.txt");
  char[] cbuf = new char[100]; // 100개씩 읽을 수 있다.
  int readData;
  while((readData = fr.read(cbuf)) != -1) {
      ...
  }
  fr.close();
  ```

  다음은 문자 출력 스트림(~Writer)의 write()메소드에 대해서 알아 봅시다.
  매개변수로 int형이 주어지면 4바이트를 보내는 것이 아니라 끝에 있는 문자 2바이트(한개 문자)를 출력스트림으로 보냅니다.

  ```java
  // 배열을 사용하지 않은 경우
  FileWriter fr = new FileWriter("C:/temp/test.txt");
  char[] data = "Java".toCharArray();
  for(int i=0; i<data.length; i++) {
    fr.write(data[i]); // J, a, v, a 한 문자씩 출력 스트림에 보낸다.
  }
  fr.flush(); // 버퍼에 잔류하는 데이터들 출력스트림으로 보내고 버퍼 비우기
  fr.close(); // 문자 출력 스트림 닫기

  // 배열을 사용하여 문자를 한꺼 번에 보내기
  FileWriter fr = new FileWriter("C:/temp/test.txt");
  char[] data = "Java".toCharArray();
  fr.write(data);
  fr.flush(); // 버퍼에 잔류하는 데이터들 출력스트림으로 보내고 버퍼 비우기
  fr.close(); // 문자 출력 스트림 닫기
  ```

  -----------------------------------------------------------------------------

  다음은 우리가 지금까지 콘솔창에서 데이터를 입력 받을 수 있었던 이유는 `콘솔 입출력`을
  사용했기 때문입니다.

  자바에서는 입력을 받기위해 System.in 스캐너에서 Scanner scan = new Scanner(System.in);
  이렇게 사용했기때문에 콘솔창에서 입력을 받을 수 있었던 것입니다. 그리고 위에서 배운
  [대용량 입출력](https://baekjungho.github.io/java-inputoutput/)에서
  BufferdReader br = new BufferdReader(new InputStreamReader(System.in)); String line = br.readLine();
  이런 식으로 사용했던 것처럼 콘솔로 입력을 받기 위해서는 `System.in`을 사용해야 합니다.
  그리고 콘솔에 데이터를 출력하기 위해서는 `System.out`을 사용했는데 System.out.println("")으로
  콘솔로 출력할 수 있었던 이유입니다. 그리고 에러를 출력할 때에는 `System.err`를 사용합니다.

  > System.out.println("") = PrintStream ps = System.out; ps.println("");

  ```java
  InputStream is = System.in;
  int number = is.read();

  Scanner scan = new Scanner(System.in);

  BufferedReader br = new BufferdReader(new InputStreamReader(System.in));
  String line = br.readLine();
  ```

  read()메소드는 1바이트만 읽기 때문에 아스키 코드로 표현될 수 있는 특수문자, 영어, 숫자는
  읽지만, 한글은 배열로 만들어서 읽어야 합니다. 한글은 ANSI 기준으로 2바이트이고
  UTF-8기준으로 3바이트 입니다.

  따라서 바이트 배열에는 `아스키 코드` 값이 저장됩니다. 이것을 사용하기 위해서는 문자열로 변환 해야 하고
  변환할때 읽은 바이트수 -2를 해줘야 합니다. 왜냐하면 Enter키는 캐리지리턴(13) + 라인피드(10)이기 때문입니다.

  ```java
  byte[] byteData = new byte[15];
  int readByteNo = System.in.read(byteData);
  // (바이트배열, 시작인덱스, 읽은 바이트수-2)
  String str = new String(byteData, 0, readByteNo-2);
  ```

  write()로 출력을 할 수 있는데 아스키 코드를 문자로 출력합니다.

  ```java
  OutputStream os = System.out;
  for(byte b = 48; b<58; b++){
    os.write(b); // 아스키 코드 48~57 출력
  }
  os.write(10); // 라인피드(줄바꿈)

  String str = "프로그래밍 공부는 꾸준히";
  byte[] byteData = str.getBytes();
  os.write(byteData);
  os.flush();
  ```

  -----------------------------------------------------------------------------

  다음은 파일 입출력에 대해 배워봅시다.

  File 클래스는 `파일크기, 속성, 이름` 등의 정보를 얻어 낼 수 있고 파일 삭제, 생성및 디렉터리 생성이 가능합니다.
  단, `파일의 데이터를 읽고 쓰는 기능은 지원하지 않습니다.`
  파일 경로를 표시할때에는 역슬래시 두 번(\\)이나, 슬래시(/)를 사용합니다.

  [파일 및 디렉터리 생성 예제](https://baekjungho.github.io/question-solving2)

  ```java
  File file = new File("C:/temp/test.txt");
  // boolean isExist = file.exists(); // 해당경로에 파일이 있는지 존재
  if(!file.exists()) { // 해당 경로에 파일이 존재하지 않으면
    file.createNewFile();
  }
  ```

  다음 메소드들은 리턴타입이 전부 boolean입니다.

  __createNewFile() 새로운 파일 생성__

  __mkdir() 디렉터리 생성__

  __mkdirs() 경로상에 없는 모든 디렉터리 생성__

  __delete() 파일 또는 디렉터리 삭제__

  다음은 파일을 바이트 단위로 읽어오는 FileInputStream에 대해 배워 봅시다.
  바이트 단위로 읽어오기 때문에 문자, 숫자, 그림, 텍스트 파일 등 전부 가능합니다.
  파일 입력 스트림도 read()메소드를 지원합니다.

  ```java
  FileInputStream fis = new FileInputStream("C:/temp/test.txt");
  // File file = new File("C:/temp/test.txt");
  // FileInputStream fis = new FileInputStream(file);
  int readByteNo;
  byte[] byteData = new byte[100];
  while((readByteNo = fis.read(byteData)) != -1) { // 100 바이트씩 처리
      ...
  }
  fis.close();
  ```

  다음은 파일 출력 스트림인 FileOutputStream입니다. write()메소드를 똑같이 지원합니다.

  ```java
  FileOutputStream fos = new FileOutputStream("C:/temp/test.txt");
  // File file = new File("C:/temp/test.txt");
  // FileOutputStream fos = new FileOutputStream(file);
  byte[] data = "Java".getBytes(); // 문자열을 바이트 배열로 변환
  fos.write(data); // 바이트 배열 data를 파일 출력 스트림으로 보낸다.
  fos.flush(); // 버퍼에 잔류하는 데이터 보내고 비우기
  fos.close(); // 파일 출력 스트림 닫기
  ```

  파일 입력스트림과 파일 출력스트림을 같이 사용하면 `파일 복사`를 할 수 있습니다.

  ```java
  String path = "C:/temp/test.txt";
  FileInputStream fis = new FileInputStream(path);
  FileOutputStream fos = new FileOutputStream(path);

  int readByteNo; // 실제로 읽은 바이트 수
  byte[] byteData = new byte[100]; // 바이트 저장

  while((readByteNo = fis.read(byteData)) != -1) {
    fos.write(byteData, 0, readByteNo); // 파일 복사
  }
  fis.close();
  fos.flush();
  fos.close();
  ```

  다음은 파일 문자 입력 스트림입니다. 텍스트 파일만 처리 가능하고 비디오, 오디오 등은 불가능 합니다.
  입력스트림이기 때문에 read()메소드를 지원합니다.

  ```java
  FileReader fr = new FileReader("C:/temp/test.txt");
  // File file = new File("C:/temp/test.txt");
  // FileReader fr = new FileReader(file);

  char[] cbuf = new char[100];
  int readCharNo;
  while((readCharNo = fr.read(cbuf)) != -1) {
      String data = new String(cbuf, 0, readCharNo); // 콘솔에 텍스트 파일 출력
      System.out.print(data);
  }
  fr.close();
  ```

  다음은 파일 문자 출력스트림인 FileWriter입니다. 출력스트림이기 때문에 write()메소드를 지원합니다.
  한글로 된 문자열을 바로 출력할 수 있습니다.

  ```java
  FileWriter fw = new FileWriter("C:/temp/test.txt");
  String str = "Java";
  fw.write(str); // 문자 출력 스트림으로 보내기
  fw.flush();
  fw.close();
  ```

  -----------------------------------------------------------------------------

  다음은 보조스트림 입니다. 보조스트림은 입출력스트림과 함께 연결되어 사용됩니다.
  예를 들어 문자 변환 보조스트림인 InputStreamReader를 BufferdReader에 추가하여 사용하는 방법은 다음과 같습니다.

  문자 변환 보조 입력스트림(InputStreamReader), 문자 변환 보조 출력스트림(OutputStreamWriter)이 있습니다.

  ```java
  InputStream is = System.in;
  InputStreamReader reader = new InputStreamReader(is);
  BufferdReader br = new BufferdReader(reader);

  // 한 줄 코드로 나타내기
  BufferdReader br = new BufferdReader(new InputStreamReader(System.in));
  String line = br.readLine(); // 한 줄씩 입력 받기
  ```

  파일 문자 입력스트림을(FileReader) 성능 향상 보조 스트림인 BufferdReader에 추가 하여 파일을 라인 단위로
  읽어 들일 수 있습니다.

  ```java
  FileReader fr = new FileReader("C:/temp/test.txt");
  BufferedReader br = new BufferdReader(fr);
  ```

  파일 출력을 위한 FileOutputStream을 Writer 타입으로 변환 할 수 있습니다.

  ```java
  FileOutputStream fos = new FileOutputStream("C:/temp/test.txt");
  Writer writer = new OutputStreamWriter(fos);
  ```

  [성능 향상 보조스트림](https://baekjungho.github.io/java-basic6)에는 BufferedInputStream(바이트 입력 스트림에 연결)과
  BufferdReader(문자 입력 스트림에 연결)가 있습니다. 출력에는 BufferedWriter와 BufferedOutputStream이 있습니다.

  둘 다 내부 버퍼 크기만큼 데이터를 미리 읽고 버퍼에 저장해 둡니다.
  그리고 버퍼로 부터 데이터를 읽어서 속도를 향상 시킵니다.

  ```java
  FileInputStream fis = new FileInputStream("C:/temp/test.txt");
  BufferedInputStream bis = new BufferedInputStream(fis);
  while(bis.read() != -1) {
      ...
  }
  ```

  다음은 기본타입 입출력 보조 스트림입니다. DataInputStream과 DataOutputStream이 있으며,
  boolean, int, float, char과 같은 기본 타입으로 입출력 할 수 있습니다.

  ```java
  FileOutputStream fos = new FileOutputStream("C:/temp/test.txt");
  DataOutputStream dos = new DataOutputStream(fos);

  dos.writeUTF("자바");
  dos.writeInt(10);
  dos.flush();
  dos.close();

  FileInputStream fis = new FileInputStream("C:/temp/test.txt");
  DataInputStream dis = new DataInputStream(fis);

  for(int i=0; i<1; i++){
    String name = dis.readUTF();
    int number = dis.readInt();
    System.out.println(name + ", " + number);
  }
  ```

  다음은 [프린터 보조 입출력 스트림](https://baekjungho.github.io/java-iostream/)인 PrintStream과 PrintWriter입니다.
  PrintStream은 바이트 출력 스트림과 연결되고 PrintWriter는 문자 출력 스트림과 연결됩니다.
  우리가 print()와 println()을 사용할 수 있었던 것은 `System.out이 PrintStream 타입`이기 때문입니다.

  ```java
  FileOutputStream fos = new FileOutputStream("C/temp/test.txt");
  PrintStream ps = new PrintStream(fos);

  ps.println("파일에 내용을 작성"); // 파일로 해당 문자열을 출력
  ps.print("Java");
  ps.flush();
  ps.close();
  fos.close();
  ```

## 대용량 입출력

  대용량 입출력을 위해서는 `BufferedReader(입력) & BufferedWriter(출력)`을 사용합니다.

  - __Memory Buffer__

    `메모리 버퍼(buffer)`는 프로그램이 데이터를 직접 하드디스크에 보내지 않고, 메모리 버퍼에
    보내고 메모리 버퍼는 데이터가 꽉차게 되면 한꺼번에 하드디스크를 보냄으로써 성능을 향상 시킵니다.

  - __BufferedReader__

    Scanner를 통해 입력을 받을경우 Space와 Enter를 모두 경계로 인식하기에 입력받은 데이터를 가공하기 매우 편리합니다.
    반면, BufferedReader는 Enter만 경계로 인식하고 받은 데이터가 `String으로 고정` 되기때문에 입력받은 데이터를 가공하는 작업이 필요할경우가 많습니다. Scanner에 비해 다소 사용하기 불편합니다. 하지만 __많은 양의 데이터를 입력받을경우 BufferedReader를 통해 입력받는 것이 효율면에서 훨씬 낫습니다.__ 입력시 Buffer 메모리줌으로써 작업속도 차이가 많이납니다.

    ```java
    BufferedReader bf = new BufferedReader(new InputStreamReader(System.in)); //선언
    String str = bf.readLine(); //String
    int number = Integer.parseInt(bf.readLine()); //Int
    ```

    BufferedReader에 연결된 InputStreamReader는 BufferedReader와 같은 `보조스트림`이며 둘 다
    소스 스트림이 `바이트 스트림(InputStream, OutputStream, FileInputStream, FileOutputStream)`이며
    입출력 데이터가 문자일 경우 Reader와 Writer로 변환하여 사용합니다.

    만약 다른 타입을 입력받고 싶은경우는 String으로 받고나서 형 변환을 해주어야 합니다.
    그리고 BufferedReader와 readLine()메소드 사용시 `try-catch`를 이용한 예외처리를 꼭 해주어야 합니다.
    아니면 `throws Exception`으로 처리해주면 됩니다.

    Scanner는 공백(Space)을 경계로 인식해서 문자열을 구분 지을 수 있는데 BufferedReader는 Line단위로 구분짓기 때문에
    공백으로 문자열을 나누려면 `가공 작업`이 필요합니다.

    ```java
    StringTokenizer st = new StringTokenizer(str); // StringTokenizer를 사용
    // StringTokenizer st = new StringTokenizer(str, " ");
    int a = Integer.parseInt(st.nextToken()); // 첫번째 호출
    int b = Integer.parseInt(st.nextToken()); // 두번째 호출

    // String 클래스의 split()메소드 사용 공백마다 데이터 끊어서 배열에 넣음
    String array[] = s.split(" ");
    ```

  - __BufferedWriter__

    일반적으로 출력할때 System.out.println();을 사용하지만 많은 양을 출력할때는 buffer를 사용해야 성능적으로 더 좋게 처리할 수 있습니다.

    ```java
    BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));//선언
    String s = "abcdefg";// 출력할 문자열
    bw.write(s+"\n");// 출력
    bw.flush();// 남아있는 데이터를 모두 출력시킴
    bw.close();// 스트림을 닫음
    ```

    write()메소드는 줄바꿈을 해주지 않기때문에 \n으로 줄바꿈을 처리 해주어야 합니다.

    그리고 BufferedWriter는 buffer를 잡아 놓기 때문에 마지막에 `flush()와 close()`로 처리해야 합니다.

  > Reference : [Coding Factory](https://coding-factory.tistory.com/251)
