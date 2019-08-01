---
title: "Memory of Java"
layout: post
category: Java
tags: [Java]
excerpt: "Java의 메모리 구조에 대해 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

자바 공부를 하면서 참고한 서적입니다.

_명품자바에센셜(생능출판사, 황기태), 이것이 자바다(한빛미디어, 신용권)_

## 자바의 메모리 구조

  JVM은 OS로 부터 메모리를 받아 관리합니다.

  이 메모리 공간을 `Runtime Data Area`라고 합니다.

  이 메모리 공간을 총 5개로 나누어 관리하고 크게는(메소드, 스택, 힙) 3영역으로 나누어 관리합니다.

  ![memory1](/images/posts/201903/memory1.jpg)

  - 메소드(Method, static, class Area)영역

  메소드, 스태틱, 클래스 영역이라고 불립니다.

  자바 컴파일러가 자바 소스 프로그램을 컴파일한 기계어를 `바이트 코드`라고 하는데

  이 메소드 영역에 `바이트 코드`가 로드가 되고 JVM에 의해 번역, 실행 됩니다.

  생성자, 메소드, static 변수, 멤버 변수 등이 저장됩니다.

  - Runtime Constant Pool

  메소드 영역에 포함되며, 클래스 파일 constant_pool 테이블에 해당하는 영역입니다.

  클래스와 인터페이스 상수, 메소드와 필드에 대한 모든 레퍼런스를 저장하고

  JVM은 런타임 상수 풀을 통해 해당 메소드나 필드의 실제 메모리 상 주소를 찾아 참조합니다.

  - Heap Area

  JVM이 관리하는 프로그램 상에서 데이터를 저장하기 위해 런타임 시 동적으로 할당하여 사용하는 영역입니다.

  힙 영역에는 객체나, 배열이 저장되고 참조하는 변수나 필드가 없으면 GC(Garbage Collector)의
  대상이 되어 자동적으로 메모리 관리를 해줍니다.

  (객체안에 배열이 포함됩니다.)

  - Stack Area

  스택은 FILO(First In Last Out) 선입후출 방식으로 push, pop의 구조를 사용 합니다.

  메소드를 호출 할 때 마다 Frame(프레임)을 추가(push)하고 메소드 종료시 pop(삭제)합니다.

  JVM의 스택 영역은 Thread(스레드)마다 한개 씩 존재하면 스레드가 시작할 때 할당됩니다.

  메소드가 호출될 때 메모리에 할당되고 종료되면 메모리가 해제됩니다.

  - PC Register

  현재 수행 중인 JVM 명령 주소를 갖습니다. 프로그램 실행은 CPU에서 인스트럭션(Instruction)을 수행합니다.

  CPU는 인스트럭션을 수행하는 동안 필요한 정보를 CPU 내 기억장치인 레지스터에 저장합니다.

  연산 결과값을 메모리에 전달하기 전 저장하는 CPU 내의 기억장치

  - Native Method Stack Area

  자바 외 언어로 작성된 네이티브 코드를 위한 Stack입니다.

  즉, JNI(Java Native Interface)를 통해 호출되는 C/C++ 등의 코드를 수행하기 위한 스택입니다.

  네이티브 메소드의 매개변수, 지역변수 등을 바이트 코드로 저장합니다.

  네이티브 메소드는 OS의 시스템 정보, 리소스를 사용하거나 접근하기 위한 코드입니다. 또한
  JNI(Java Native Interface) API를 사용하면 자바 프로그램에서 OS 시스템에 대한 접근이 가능합니다.
  이 때 사용되는 네이티브 메소드들에 대한 정보가 네이티브 메소드 스택 영역에 저장됩니다.

## Check memory usage

  자바로 프로그램을 개발하다 보면 메모리 사용량을 체크할 때가 있습니다.

  java.lang.Runtime 클래스에서 메모리 확인을 할 수 있는 클래스 메소드(static method)를 제공합니다.

  1. freeMemory() : JVM에서 사용 가능한 메모리의 양을 반환 합니다.

  2. maxMemory() : JVM이 사용하려는 최대 메모리 양을 반환 합니다.

  3. totalMemory() : JVM의 총 메모리양을 반환 합니다.

  > 사용방법
  >
  > 사용한 메모리 양 구하기 : totalMemory() - freeMemory();
  >
  > Runtime.getRuntime().gc(); // gc() = garbage collection을 실행합니다.
  >
  > 사용전에 위 코드를 작성하면 조금 더 정확하게 메모리 사용량을 알 수 있습니다.
  >
  > 메모리 반환 단위는 Byte입니다.
  >
  > 만약에 메모리양을 MB단위로 나타내고 싶으면 1024로 2번 나눠 주면 됩니다.
  >
  > Byte - KB - MB - GB - TB 각 단계별로 1024로 나눠주면 됩니다.
  >
  > 클래스 메소드 사용방법은 Runtime.getRuntime().freeMemory() 방식으로 사용합니다.

  ```java
  import java.util.Scanner;
  public class PrintNumberOneLineExample {
  	public static void main(String[] args) {
  		// static Runtime run = Runtime.getRuntime()
  		// run.totalMemory() 방식으로도 접근 가능
  		Runtime.getRuntime().gc(); // 가비지 컬렉션 실행

  		int mb = 1024*1024; // byte kb mb gb 순서이므로 mb로 나타내기 위한 변수

  		// 전체 메모리
  		System.out.println("totalMemory " + Runtime.getRuntime().totalMemory()/mb + "MB");

  		// 사용전 FreeMemory
  		System.out.println("사용 전 Free Memory " + Runtime.getRuntime().freeMemory()/mb + "MB");

  		Scanner scan = new Scanner(System.in);
  		System.out.print("자연수 N을 입력하세요 : ");
  		int number = Integer.parseInt(scan.nextLine());

  		for(int i=1; i<=number; i++) {
  			System.out.println(i);
  		}
  		// 사용한 메모리 양 구하기
  		double usageMemory = Runtime.getRuntime().totalMemory()/mb - Runtime.getRuntime().freeMemory()/mb;
  		System.out.println("사용한 메모리 " + usageMemory  + "MB");
  		System.out.println("사용 후 Free Memory " + Runtime.getRuntime().freeMemory()/mb + "MB");

  	 }
   }
   ```

   위 프로그램 실행 시 전체메모리 값과 사용전 FreeMemory값이 달랐는데 그 이유는 변수 등 데이터 들이
   메모리 안에 이미 저장되어있고, 그 만큼의 크기를 사용하고 있어서 다른거 같습니다.

   > Reference
   >
   > [https://wanzargen.tistory.com](https://wanzargen.tistory.com)
   >
   > [https://hoonmaro.tistory.com](https://hoonmaro.tistory.com)
   >
   > [http://blog.naver.com/PostView](http://blog.naver.com/PostView.nhn?blogId=heartflow89&logNo=220954420688)
