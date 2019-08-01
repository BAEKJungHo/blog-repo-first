---
title: "Thread"
layout: post
category: Java
tags: [Java]
excerpt: "Java의 Thread에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

자바 공부를 하면서 참고한 서적입니다.

_명품자바에센셜(생능출판사, 황기태), 이것이 자바다(한빛미디어, 신용권), 디자인패턴과 리팩토링_

## Thread

  - Process

    `프로세스(Process)`란 운영체제에서 실행 중인 하나의 Application을 말합니다.

    > 작업관리자에 표시되는 프로세스이름 하나가, 하나의 Application

    `다중 프로세스(Multi Process)`는 크롬이나, 파이어폭스 등 브라우저를 두 개 이상 실행하면
    작업관리자에 표시되는 프로세스가 두 개 이상이 나옵니다. 즉, 프로세스가 여러개 돌아갑니다.

    `멀티태스킹(Multi Tasking)`이란 두 가지 이상의 작업을 동시에 처리하는 것을 말합니다.

    > EX) PPT를 만들면서 음악을 듣는것, 채팅(메신저)를 하면서 다운로드 받는 것

    한 프로세스 내에서 멀티태스킹이 가능한 Application들이 있는데 예를들어 미디어 플레이어와
    메신저가 대표적인 예입니다.

    이렇게 한 프로세스가 두 가지 이상의 작업을 처리하기 위해서는 `멀티 스레드(Multi Thread)`를
    사용해야 합니다.

    한 가지의 스레드만 존재하는 것을 `싱글 스레드(Single Thread)`라고 합니다.

    ![thread1](/images/posts/201904/thread1.jpg)

  - MultiThread

    멀티 스레드가 실행되는 방식은 2가지 입니다.

    - 동시성(Concurrency)

      동시성은 `코어는 1개 스레드는 여러개`라서 멀티 스레드가 번갈아 가며 실행됩니다.

    - 병렬성(Parallelism)

      병렬성은 `코어는 여러개 각 코어마다 스레드 1개` 즉, `멀티 코어, 멀티스레드`입니다.

      각각의 코어에서 스레드를 동시에 실행합니다.

  - Multi Process vs MultiThread

    멀티 프로세스 내에 존재하는 여러 프로세스들은 각각 독립적으로 실행합니다.

    반면, 각각의 프로세스 내에 존재하는 `멀티 스레드`들은 `예외(Exception)`가 발생되면 서로에게 영향을 끼칠
    수 있습니다.

  - Main Thread

    모든 자바 프로그램들은 메인 스레드(Main Thread)가 존재 합니다.

    메인 스레드는 메인 메소드를 위에서 아래로 내려가면서 코드를 실행합니다.

    메인 스레드 이외에 여러 `작업 스레드`를 만들어 `병렬`로 코드를 실행할 수 있습니다.

  -----------------------------------------------------------------------------

### 작업스레드 생성방법

  Thread 클래스는 java.lang 소속입니다.

  > java.lang.thread

  작업 스레드 객체를 생성하기 위해서는 `Runnable Interface`를 구현해서 사용해야합니다.

  Runnable에는 `run()`메소드가 하나 있습니다. 이 run()메소드는 작업 스레드가 실행할 코드를 가지며
  `구현클래스`에서 run()메소드를 오버라이딩하여 `실체메소드`를 작성해서 사용해야 합니다.

  그리고 스레드 시작을 하기 위해서는 `start()`메소드를 사용해야 합니다. `스레드객체명.start()`
  start()메소드는 run()메소드를 실행합니다.

  즉, 정리하자면 다음과 같습니다.

  1. 구현클래스 만들기 (EX) work에서 Runnable Interface 구현)
  2. 실체메소드 만들기 (run() 메소드를 오버라이딩)
  3. 스레드 객체 생성하기
  4. 스레드 실행하기 (start() 메소드)

### Runnable 구현

  - 사용방법1. (구현클래스 생성)

  ```java
  class Task implements Runnable {
    @Override
    public void run() {
      // 작업 스레드가 실행할 코드
    }
  }

  public class WorkingThreadExample {
    public static void main(String[] args) {
      Runnable task = new Task();
      Thread thread = new Thread(task);
    }
  }
  ```

  > Thread thread = new Thread(RUNNABLE INTERFACE)
  >
  > Runnable Interface를 매개변수로 갖는 생성자 호출

  - 사용방법2. (익명구현객체사용)

  ```java
  public class WorkingThreadExample {
    public static void main(String[] args) {
      Thread thread = new Thread(new Runnable() {
        pubic void run() {
          // 작업 스레드가 실행할 코드
        }
      });
    }
  }
  ```

  [익명구현객체](https://baekjungho.github.io/java-interface/#%EC%9D%B5%EB%AA%85%EA%B5%AC%ED%98%84%EA%B0%9D%EC%B2%B4)를 쉽게 이해하자면
  int a = 10; sum(a);이런 방식대신 sum(10)이라는 방식을 사용하는 것입니다.
  즉 따로 담을 변수를 만들지 않고 괄호안에 인터페이스를(new Interface)때려박고 그 안에다가
  추상메소드를 오버라이딩 하여 실체메소드를 만드는 것입니다.

  - 사용방법3. (람다식 사용)

  ```java
  public class WorkingThreadExample {
    public static void main(String[] args) {
      Thread thread = new Thread(() -> {
        // 작업 스레드가 실행할 코드
      });
    }
  }
  ```

### Thread 상속받기

  위에서 Runnable Interface를 구현하여 스레드 객체를 만드는 방법을 배웠습니다.

  이번엔 Thread 클래스를 상속받아서 작업 스레드 객체를 생성하는 방법을 배워보겠습니다.

  Thread 클래스를 상속 받더라도 똑같이 run()메소드를 오버라이딩 해줘야 합니다.

  - 상속

  ```java
  public class TaskThread extends Thread {
    @Override
    public void run() {
      // 작업 스레드가 실행할 코드
    }
  }
  ```

  - 익명구현객체

  ```java
  public class WorkingThreadExample {
    public static void main(String[] args) {
      Thread thread = new Thread() {
        pubic void run() {
          // 작업 스레드가 실행할 코드
        }
      };
    }
  }
  ```

  -----------------------------------------------------------------------------

  - __setName(), getName()__

  setName()과 getName() 메소드는 인스턴스(객체) 메소드 입니다.

  따라서 `객체명.메소드명`으로 접근해야 합니다.

  > 스레드 객체 얻기 : Thread thread = Thread.currentThread();
  >
  > currentThread() 메소드는 클래스 메소드(static method)입니다.

  Thread는 각각 자신의 이름을 가지고 있습니다. 메인스레드의 이름은 `main`이며 사용자가
  직접 생성한 스레드의 이름은 Default로 `Thread-n`으로 설정됩니다. 이름을 직접 지어 주고 싶을때
  `setName()`메소드를 사용하면 됩니다.

  > thread.setName("스레드 명");

  이름을 얻어올때는 getName()으로 얻어오면 됩니다.

  > thread.getName();

  -----------------------------------------------------------------------------

### Thread Scheduling

  `스레드 스케줄링(Thread Scheduling)`은 `동시성(Concurrency)`의 경우, 즉, 스레드 개수가
  코어개수 보다 많은 경우 스레드를 어떤 순서에 의해 실행 할 것인가를 결정하는 것입니다.

  - 자바(JVM)의 스레드 스케줄링 방식

    자바의 스레드 스케줄링 방식은 다음과 같습니다.

    - __선점형 스케줄링 방식 = 우선순위(Priority) 방식 - setPriority()__

      스레드의 우선권을 가지고 `Priority`가 높은 스레드를 먼저 수행시키는 방식

      > 개발자가 우선순위 부여가능 : 코드로 제어 가능

      우선순위는 1(가장 낮음)~10(가장 높음)까지 부여됩니다.

      우선순위 Default는 5로 정해집니다. 우선순위를 정할때는 `setPriority()`를 사용하면 됩니다.

      > thread.setPriority(Thread.MIN_PRIORITY);
      >
      > thread.setPriority(Thread.MAX_PRIORITY);

    - __순환 할당 방식(Round-Robin)__

      `시간 할당량(Time Slice)`을 정해서 하나의 스레드를 정해진 시간만큼 실행하고
      다시 다른 스레드를 실행하는 방식(`순환`)

      > JVM이 자동으로 실행 : 코드로 제어 불가능

  -----------------------------------------------------------------------------

### Synchronized

  멀티 스레드에서 `공유 객체`를 사용할 때 조심해야 하는 점이 A라는 스레드와 B라는 스레드에서
  코드를 실행하면서 같은 `메모리 번지`에 접근한다는 것입니다. 그러면 한 스레드가 멈춘 사이에
  다른 스레드가 `값을 변경`할 수 있기 때문에 `멀티 스레드`를 사용할때는 `임계영역(Critical Section)`
  을 지정해서 사용 해야 합니다. 임계영역을 지정하면은 멀티 스레드 프로그램 내에서 단 하나의
  스레드만이 접근하여 사용할 수 있고, 다른 스레드들은 접근이 불가능 합니다. 임계영역을 지정하기 위해
  `동기화 메소드, 동기화 블록`을 사용해야 합니다.

  즉, Thread A가 동기화 메소드나, 동기화 블록에 들어가게 되면 Thread B, C, D .....는
  동기화 메소드와 블록에 접근하지 못하고 일반 메소드와 일반 블록에만 접근 가능하게 됩니다.

  - __Synchronized Method & Synchronized Block__

  `동기화 메소드`는 메소드 이름 앞에 __synchronized__ 키워드를 붙여서 만드는 것이고,
  `동기화 블록`은 메소드 내부 특정 부분만 지정하여 __synchronized__ 키워드를 붙여서 만드는 것입니다.

  ```java
  public synchronized void methodA() {
    // Critical Section
  }

  public void methodB() {
    // 멀티 스레드 접근 가능

    synchronized(공유객체) {
      // Critical Section
    }

    // 멀티 스레드 접근 가능
  }
  ```

  -----------------------------------------------------------------------------

### Thread State

  스레드는 start()메소드를 사용한다고 해서 바로 실행되는 것이 아니라 `실행대기와 실행`상태를
  반복합니다. 스레드 스케줄링에 의해 실행대기와 실행을 반복하게 되며, 더 이상 실행할 코드가
  없을 경우 `종료`상태로 오게 됩니다. 또한 실행상태에서 일시정지(스레드 실행 불가능)상태로도
  갈 수 있습니다. 일시정지 상태에서 다시 실행상태로 가기 위해서는 `실행대기(Runnable)`를 거쳐야 합니다.

  > Tread Instance --> start() --> Runnable <---> Running --> Terminate
  >
  > Runnable --> WATING --> Runnable --> Running --> Terminate

  ![run](/images/posts/201904/run.jpg)

  **Thread State**  

  |상태    |ENUM|특징|
  |--------|----|----|
  |객체 생성|  NEW|  스레드 객체를 생성|
  |실행 대기|  RUNNABLE|  실행 상태로 갈 수 있는 상태|
  |일시정지| WATING|  다른 스레드가 통지할 때 까지 기다리는 상태|
  |일시정지| TIMED_WAITING|  주어진 시간동안 기다리는 상태|
  |일시정지| BLOCKED|  사용하고자 하는 객체의 락이 풀릴 때까지 기다리는 상태|
  |종료|  TERMINATED|  실행을 마친 상태|

  > 스레드 상태를 이용한 예 : 동영상 일시정지, 노래 일시정지 등

  스레드 상태를 알고 제어를 잘 할 줄 알아야 좋은 `멀티 스레드 프로그램`을 만들 수 있습니다.

  다음은 스레드 상태 제어(Thread State Control)를 담당하는 메소드들 입니다.

  **Thread State Control**  

  |메소드    |특징|
  |--------|------|
  |interrupt()|  일시 정지 상태의 스레드에서 InterruptException을 발생시켜, 예외처리 코드(catch)에서 실행 대기 상태로 가거나 종료상태로 갈 수 있게한다.|
  |notify()/notifyAll()|  동기화 블록 내에서 wait() 메소드에 의해 일시정지 상태에 있는 스레드를 실행 대기 상태로 만든다.|
  |resume()| Deprecated Mehtod - notify(), notifyAll()사용 |
  |sleep()| 주어진 시간 동안 스레드를 일시 정지 상태로 만들고 끝나면 자동으로 실행 대기 상태가 된다.|
  |join()| 스레드를 일시 정지 상태로 만들고, 실행 대기 상태로 가려면 Terminate 되거나, 매개값에 주어진 시간이 지나야한다.|
  |wait()| 동기화 블록 내에서 스레드를 일시 정지 상태로 만든다. 매개값에 주어진 시간이 지나거나 notify(), notifyAll()에 의해 실행 대기 상태로 간다.|
  |suspend()| Deprecated Method - wait()사용 |
  |yield()| 양보 메소드, 실행 중에 우선순위가 동일한 다른 스레드에게 실행을 양보하고 실행 대기 상태가 된다.|
  |stop()| 스레드를 종료시킨다. |

  위 메소드들 중에서 `사용 권장을 하지 않는 메소드(Deprecated Method)`들은 resume(), suspend(), stop()입니다.
  join(), wait(), sleep()메소드는 (long millis)/(long millis, int nanos)의 매개값을 줄 수 있습니다.

  그리고 wait(), notify(), notifyAll()은 Object 클래스의 메소드이며, 나머지는 Thread 클래스의 메소드 입니다.

  -----------------------------------------------------------------------------

### Demon Thread

  데몬 스레드(Demon Thread)는 주 스레드를 도와주는 보조 역할을 담당합니다. 따라서 주 스레드가
  종료되면 데몬 스레드도 종료됩니다.

  > EX) 동영상 플레이어의 재생

  - 데몬 스레드 생성 방법

    스레드객체명.setDaemon(true); start(); start()는 setDaemon()후에 실행 해야 합니다.

  - 데몬 스레드 구별 방법

    isDaemon()을 사용하여 데몬 스레드 일 경우 true리턴 아닐경우 false리턴

  -----------------------------------------------------------------------------

### Thread Group

  스레드 그룹(Thread Group)은 서로 연관된 스레드를 묶어서 관리할 목적으로 사용합니다.
  스레드는 반드시 하나의 스레드 그룹에 포함되며, 만약 포함 시키지 않을 경우
  Default로 자신을 생성한 스레드와 같은 그룹에 속하게 됩니다.

  - 스레드 그룹 이름 얻는 방법

    > ThreadGroup group = Thread.currentThread().getThreadGroup();
    >
    > String groupName = group.getName();

  - 프로세스에서 실행 중인 모든 스레드 정보 얻기

    정적 메소드(static method)인 `getAllStackTraces()`사용

  - 스레드 그룹 생성 방법

    > ThreadGroup group = ThreadGroup(ThreadGroup parent, String name);

    부모 ThreadGroupd은 생략 할 수 있습니다. 부모 스레드 그룹을 지정하지 않으면
    현재 스레드가 속한 그룹의 하위 그룹으로 속하게 됩니다.

  - interrupt()

    스레드들이 같은 그룹에 있을 경우 interrupt()는 한번만 사용해주면 됩니다.
    단, 예외처리는 스레드마다 처리해줘야 합니다.

  -----------------------------------------------------------------------------

### Thread Pool

  스레드 풀(Thread Pool)은 병렬 작업의 개수가 폭증할때 이 것을 막기 위해 사용됩니다.
  스레드 풀을 사용할 수 있도록 `java.util.concurrent`패키지에서 `ExecutorService`인터페이스와
  `Executors`클래스를 제공하고 있습니다. 쓰레드 풀은 동시에 가동하는 쓰레드 수의 최대치에 제한을 둘 때 유용합니다.
  그리고 각 스레드에는 각각의 스택을 위한 메모리가 할당되어야 한다.
