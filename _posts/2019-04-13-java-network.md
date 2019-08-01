---
title: "Socket Programming"
layout: post
category: Java
tags: [Java]
excerpt: "Java의 네트워크 프로그래밍 Socket에 대해 배웁니다."
author: BAEKJungHo
---

* content
{:toc}

자바 공부를 하면서 참고한 서적입니다.

_명품자바에센셜(생능출판사, 황기태), 이것이 자바다(한빛미디어, 신용권), 디자인패턴과 리팩토링_

## TCP Networking

  [Web과 Network에 대해 공부하기](https://baekjungho.github.io/web-tree)

  TCP(Transmission Control Protocol)는 연결 지향적 프로토콜 입니다. 따라서 데이터를 정확하고 안정적으로 전달할 수 있습니다.
  하지만 단점은 연결이 되어있어야 하며, UDP(User Datagram Protocol)보다 데이터 전송 속도가 느릴 수 있습니다.

  > 연결 지향적 프로토콜 : 클라이언트와 서버가 연결된 상태에서 데이터를 주고받는 것

  자바에서는 TCP Networking을 위해 java.net.ServerSocket과 java.net.Socket 클래스를 제공하고 있습니다.

  - __IP주소 얻는방법__

    자바는 IP주소를 java.net.InetAddress 객체로 표현합니다. InetAddress는 로컬 컴퓨터의 IP주소, DNS에서 도메인 이름에 대한 IP 주소를 가져오는 기능을 가지고 있습니다. InetAddress 클래스를 사용할때에는 `UnknownException` 예외처리를 해주어야 합니다.


    > InetAddress ia = InetAddress.getLocalHost(); // 로컬 컴퓨터의 IP주소 얻기

    도메인 이름을 통한 IP주소를 얻는 방법은 다음과 같습니다.

    ```java
    try {
      // getByName() : DNS에 등록된 하나의 IP주소 얻기
      InetAddress ia = InetAddress.getByName(String host);
      // getAllByName(): DNS에 등록된 모든IP주소 얻기
      InetAddress[] iaArr = InetAddress.getAllByName("www.google.com");

      // google의 ip주소 얻기
      for(InetAddress remote : iaArr) {
        System.out.println(remote.getHostAddress());
      }

      // getHostAddress() : InetAddress 객체에서 IP주소 얻기
      String ip = InetAddress.getHostAddress();
    } catch(UnknownException e) {
      e.printStackTrace();
    }
    ```

    위 처럼 try-catch를 통해 UnknownException 예외처리를 꼭 해주어야 합니다.

## Socket

  TCP Networking 프로그래밍을 하기 위해서는 [서버와 클라이언트의 작동방식](https://baekjungho.github.io/web-tree)에 대해 알아야합니다.

  자바에서는 서버가 실행되면 클라이언트가 서버의 IP주소와 바인딩 포트를 통해 클라이언트에서 Socket을 하나 생성하고 서버(Server)에 있는 ServerSocket에 연결요청을 하여 연결 수락을(ServerSocket) accept()메소드로 하고 나서 서버에서 통신용 Socket을 하나 만들어 클라이언트의 Socket과 서버의 Socket을 통해 통신을 하게 됩니다.

  ![socket](/images/posts/201904/socket.jpg)

  > 연결수락 : ServerSocket / 클라이언트와 통신 : Socket
  >
  > Binding Port : 서버가 시작할때 고정된 포트번호를 가지고 시작하는것

  __클라이언트가 서버에 연결요청을 할 때 포트 번호가 필요__ 하므로 ServerSocket을 생성할 때
  포트 번호를 하나 지정해야 합니다. 이것을 바인딩 포트라고 합니다.

  즉, 자바로 TCP Networking 하는 과정은 다음과 같습니다.

  1. 서버실행
  2. 서버IP, Binding Port를 가지고 클라이언트 Socekt 생성
  3. 서버에 연결요청
  4. ServerSocket이 accept()메소드를 통해 연결 수락
  5. 서버에 클라이언트와 통신할 Socket 생성
  6. 서버와 클라이언트가 각자의 Socket을 통해 통신   

  다음 과정을 자바코드를 통해 구현하는 방법을 배워 보겠습니다.

  - __ServerSocket 생성과 연결 수락__

    서버를 개발하기 위해서 ServerSocket을 얻어야 하는데 생성자에 바인팅 포트를 넣어
    객체를 생성하면 됩니다. 바인팅 포트번호는 int형 입니다. 다른 방법은 기본 생성자를 호출하고
    bind()메소드를 통한 포트 바인딩을 하면 됩니다. 단, bind()메소드의 매개값은
    포트정보를 가지고 있는 `InetSocketAddress`객체 입니다.

    ```java
    try {
      // 바인딩 포트번호 5001이라 가정
      // 바인딩 포트번호를 가지는 생성자 호출하여 객체 초기화
      ServerSocket serverSocket = new ServerSocket(5001);

      // 기본 생성자를 호출하여 객체 초기화 하고
      // bind() 메소드로 매개값으로 포트번호를 가지는 InetSocketAddress객체 주기
      ServerSocket serverSocket = new ServerSocket();
      serverSocket.bind(new InetSocketAddress("localhost", 5001));

      // 클라이언트 연결 수락을 위해 accept()메소드 호출
      // 클라이언트가 연결 요청을 하면 Socket을 만들고 리턴
      // 클라이언트가 연결 요청하기 전까지 블로킹(Blocking)된다.
      // Blocking : 스레드 대기 상태
      // 따라서 UI, 이벤트 처리 스레드에서 accept()메소드를 호출하면 안된다.
      Socket socket = serverSocket.accept();

      // 연결된 클라이언트 IP번호 얻기
      // getRemoteSocketAddress는 실제 리턴값이 InetSocketAddress 객체 이므로 타입변환
      InetSocketAddress isa = (InetSocketAddress)socket.getRemoteSocketAddress();
      System.out.println(isa.getHostName());
    } catch(Exception e) { }

    // ServerSocket이 닫혀있지 않을경우
    if(!serverSocket.isClosed()) {
      try {
      serverSocket.close();
    } catch(IOException e) {}
    }
    ```

    만약에 IP가 여러개여서(멀티 IP) 특정 IP에만 연결하고 싶은 경우 localhost대신
    정확한 IP주소를 주면 됩니다.

    Socket을 사용할때는 예외 처리를 해주어야 합니다. 그리고 클라이언트 연결 수락이 필요 없으면
    ServerSocket을 close() 해주어야 합니다.

    ServerSocket을 생성할때 이미 사용중인 포트를 적게되면 `BindException`이 발생,
    accept()메소드를 사용할때 블로킹 상태에서 close()를 해버리면 `SocketException`이 발생

    __ServerSocket과 Socket은 블로킹 방식(동기 방식)으로 구동됩니다.__

    다음은 가장 일반적으로 사용되는 ServerSocket생성 및 연결 수락 코드 입니다.

    ```java
    import java.io.IOException;
    import java.net.InetSocketAddress;
    import java.net.ServerSocket;
    import java.net.Socket;

    public class ServerExample {
	  public static void main(String[] args) {
		ServerSocket serverSocket = null;

		try {
			serverSocket = new ServerSocket();
			serverSocket.bind(new InetSocketAddress("localhost", 5001));
			// 반복으로 accept() 호출하여 다중 클라이언트 연결 수락
			while(true) {
				System.out.println("[Connection Waiting]");
				Socket socket = serverSocket.accept(); // ServerSocket이 accept()로 연결 수락하면 Socket객체 생성
				InetSocketAddress isa = (InetSocketAddress)socket.getRemoteSocketAddress();
				System.out.println("[Connection Accept]" + isa.getHostName());
			}
		} catch(Exception e) {}

		if(!serverSocket.isClosed()) {
			try {
				serverSocket.close();
			} catch(IOException ee) {}
		  }
  	 }
    }
    ```

  - __InetSocketAddress Method__

    |메소드명   |리턴타입|특징|
    |-------|----|----|
    |getHostName()|String|  클라이언트 IP 리턴|
    |getPort()|int|  클라이언트 포트번호 리턴|
    |toString()|String|  "IP:포트번호" 형태의 문자열 리턴|

  - __Socket 생성과 연결 요청__

    서버를 만들고 나서 이제 클라이언트에서 Socket을 생성하여 연결요청을 하면
    서버의 accept()메소드가 blocking이 풀리면서 연결을 수락하게 됩니다.

    ```java
    import java.net.Socket;
    public class ClientExample {
      public static void main(String[] args) {
        Socket socket = null;
        try {
          socket = new Socket(); // Socket 생성
          System.out.println("[Connection Request]");
          socket.connect(new InetSocketAddress("localhost", 5001)); // 연결 요청
          // socket = new Socket("localhost", 5001);
          System.out.println("[Success Connection]");
        } catch(Exception e) {}

        if(!socket.isClosed()) {
          try {
            socket.close();
          } catch(IOException e) {}
        }
      }
    }
    ```

    위 코드에서 `socket.connect(new InetSocketAddress("localhost", 5001));`이 방법을 사용하는 이유는
    도메인명만 알고 있을 경우 DNS를 통해 IP주소를 얻어와야 하므로 InetSocketAddress객체를 사용하는 것입니다.

    연결 요청을 할 때 잘못된 IP주소를 입력하면 `UnknownHostException`이 발생하고
    `IOException`은 주어진 포트로 접속할 수 없을때 발생 됩니다.  
    Socket의 `connect()`메소드도 서버와 연결이 될 때 까지 블로킹(Blocking)됩니다.
    연결이 완료되고나서 프로그램을 종료하거나, 연결을 끊고 싶으면 close()메소드를 호출하면 됩니다.

  즉, 자바 코드로 TCP Networking를 구현하는 과정은 다음과 같습니다.

  - __서버 프로그램__

    1. 서버 개발을 위하여 ServerSocket생성
    2. ServerSocket의 bind() 메소드로 클라이언트와 통신할 수 있도록 포트 바인딩 하기
    3. Socket socket = serverSocket.accept()
    4. ServerSocket의 accept()메소드로 연결 수락 후 클라이언트 소켓과 통신할 수 있는 소켓 객체 생성
    5. 클라이언트 IP, 포트번호를 얻을 수 있는 InetSocketAddress객체 생성
    6. 서버 소켓 닫기

  - __클라이언트 프로그램__

    1. 서버에 연결 요청을 하고, 통신할 수 있는 Socket 생성
    2. Socket의 connect()메소드로 연결요청
    3. 클라이언트 소켓 닫기

  실행 순서는 서버 프로그램 - 클라이언트 프로그램 순서로 실행하면 됩니다.

  -----------------------------------------------------------------------------

### Communication

  클라이언트가 connect() 하고 서버가 accept() 하였으면, 각각의 Socket으로 부터
  입출력스트림(InputStream, OutputStream)을 얻을 수 있습니다.

  ```java
  // 입력 스트림 얻기
  InputStream is = socket.getInputStream();
  OutputStream os = socket.getOutputStream();
  ```

  상대방과 데이터를 보내고 받는 방법은 byte[]배열을 생성하고 OutputStream의 write()와
  InputStream의 read()를 호출하면됩니다.

  ```java
  // 데이터 보내기
  String data = "message";
  byte[] bytes = data.getBytes("UTF-8");
  OuputStream os = socket.getOutputStream();
  os.write(bytes);
  os.flush();

  // 데이터 받기
  byte[] bytes = new byte[100]; // 받은 데이터 저장
  InputStream is = socket.getInputStream();
  int len = is.read(bytes);
  String data = new String(bytes, 0, len, "UTF-8");
  ```

  InputStream의 read()메소드는 상대방에 데이터를 보내기 전까지 블로킹 됩니다.
  read()를 호출하고나서 나올 수 있는 결과는 3가지 입니다.

  - __read()__

    |경우의 수|리턴값|
    |---------|----|
    |상대방이 데이터를 보냄| 읽은 바이트 수|
    |상대방이 정상적으로 Socket의 close()호출| -1|
    |상대방이 비정상적으로 종료|IOException 발생|
