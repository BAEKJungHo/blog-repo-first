---
title: "연산, 조건문, 반복문"
layout: post
category: Java
tags: [Java]
excerpt: "Java의 연산, 조건문, 반복문에 대해 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

자바 공부를 하면서 참고한 서적입니다.

_명품자바에센셜(생능출판사, 황기태), 이것이 자바다(한빛미디어, 신용권)_

## 구조(Structure)

  자바 코드의 구조에 대해서 알아보는 시간을 가지겠습니다.

  메소드는 기능, 멤버함수라고도 하며, 속성은 필드, 멤버변수라고도 합니다.

  ![structure](/images/posts/201903/structure.jpg)

## 메소드(Method) 사용방법

  자바에서 메소드(Method)를 사용하기 위해서는 클래스 객체를 생성하고 그 객체명으로 메소드에 접근하여 사용

  > ClassName ObjectName = new ClassName() 방식으로 ObjectName의 객체 생성
  >  
  > ObjectName.MethodName() // 객체명.메소드이름으로 접근
  >
  > 결과 : ClassName이라는 class안에 있는 MethodName()의 메소드를 사용하는 코드

  ```java
  public class UsingMethod {
    public static int add(int num1, int num2){
      return num1 + num2;
    }
    public static void main(String[] args){
      // ClassName ObjectName = new ClassName()
      UsingMethod sum = new UsingMethod();

      // ObjectName.MethodName()
      System.out.println(sum.add(10, 10));
    }
  }
  ```

## 연산(Operation)

  [연산자의 종류](https://designjava.tistory.com/8?category=771497)

  자바 연산에서 숫자+숫자=숫자, 숫자+문자=문자이다. 또한 숫자+숫자+문자에서는 앞의 숫자2개가 먼저 계산되고
  계산된 숫자와 문자가 연산되어 문자라는 결과가 나옵니다.

  > 자바소스파일 : .java
  >
  > 컴파일 후 : .class

  이클립스에서 코드를 작성하고 ctrl + s로 저장하면 자동으로 컴파일이 됩니다.

  __TIP__

  System.out.println("")을 빨리 작성하는방법은 sysout을 입력하고 `ctrl+space bar`를 클릭하면 됩니다.

### 데이터 타입(Data Type)

  데이터 타입에는 기본타입 8종류와, 래퍼런스타입 1종류를 포함해서 총 9종류가 있습니다.

  추가적으로 문자열(String)리터럴과, null리터럴이 있습니다.

  **기본타입**  

  |종류    |특징|
  |--------|----|
  |byte|  1byte를 나타내며, 색상 정보 및 이미지 등을 처리할때 주로 사용|
  |char|  2byte를 나타내며, 문자(단어한개)를 나타낸다|
  |short| 2byte를 나타내며, 정수형이며 자주사용하지는 않는다.|
  |int|  4byte를 나타내며, 가장 많이 사용하는 정수형이다.|
  |long|  8byte를 나타내며, int보다 큰 값을 나타낼때 뒤에 L을 붙여야한다.|
  |float|  4byte를 나타내며, 실수형이다|
  |double|  8byte를 나타내며, 가장 많이 사용하는 실수형이다.|
  |boolean|  1byte이며, true, false를 반환한다.|

  > long 타입을 사용할때는 만약 변수에 넣을 값이 int 타입의 범위보다 클 경우 숫자뒤에 L, l을 붙여야합니다.
  >
  > long num1 = 100000000000L;
  >
  > float형 또한 뒤에 f를 붙입니다.

  **래퍼런스타입**  

  |종류    |특징|
  |--------|----|
  |배열|  배열일때 래퍼런스타입 사용|
  |클래스|  클래스일때 래퍼런스타입 사용|
  |인터페이스| 인터페이스일때 래퍼런스타입 사용|
  |열거|  클래스 처럼 정의하는 상수, 연관된 상수의 집합|

  **String, null**  

  |종류    |특징|
  |--------|----|
  |String| 기본타입이 아니며, 클래스타입 입니다. JDK에서 제공하는 String 클래스를 이용|
  |null|  int x = null; 처럼 기본타입에 사용불가능, 참조타입에서만 사용 가능|

### 참조타입vs기본타입

  기본타입은 변수안에 `실제 값`을 저장하지만, 참조타입은 `메모리 번지 값`을 가집니다.

  아래에 나와 있는 `문자열 비교 파트`에서 String 은 자바에서 제공하는 클래스이며, 참조타입 이므로

  메모리 번지를 값으로 가집니다. 따라서 문자열 비교를 할때 str1 == str2로 비교를 하는 것은

  메모리 번지가 같은지를 비교하는 것이지 절대, 문자열 그 자체가 같냐를 비교하는 것이 아닙니다.

  또한, 기본타입은 타입 첫 글자가 소문자이고, 참조타입은 첫 글자가 대문자 입니다.

  -----------------------------------------------------------------------------

  _스택에는 변수가, 힙에는 객체가 저장됩니다._

  _스택에는 메모리 주소가 저장되고, 메모리 주소가 가리키는 실제 값은 힙에 저장됩니다._

  ![m1](/images/posts/201903/m1.jpg)

  > 다음 예시를 통해서 이해 해봅시다.
  >
  > String name = "Java";
  >
  > 위에서 보았듯이 String은 참조타입이며, name은 변수, Java는 문자열 리터럴인 객체입니다.
  >
  > 참조타입 변수명 = "문자열 리터럴 객체";
  >
  > name은 변수이므로 스택(Stack)에 저장되고, "Java"는 객체이므로 힙(Heap)에 저장됩니다.
  >
  > 그리고 참조타입은 메모리 번지 값을 가진다고 했으므로 name 변수는 힙(Heap)에 저장된
  > "Java"객체의 메모리 번지 값을 가집니다.
  >
  > 즉, name 안에는 `Java객체 메모리번지`가 들어가 있습니다.
  >
  > 이때 변수 name은 new연산자를 통해서 실제 데이터가 저장된 Heap영역의 참조값을 리턴 받습니다.
  >
  > String name = new String("JAVA")와 위의 String name = "Java";는 메모리 번지가 다릅니다.
  >
  > String name = new String("JAVA")와 위의 new로 생성한 자바도 주소가 다릅니다.
  >
  > new 연산자는 `새로운 주소`를 만들어 스택에는 주소를 저장하고 객체는 힙에 저장한다.
  >
  > 따라서 문자열1 == 문자열2 이런 식의 비교는 메모리 번지 값이 같냐라는 질문입니다.

  그리고 힙(heap)이 부분이 가비지컬렉터(gc)의 대상이 되는 [메모리](https://baekjungho.github.io/java-memory/) 영역입니다.

  null은 기본타입에서는 사용 불가능하고, 참조타입에서만 사용 가능하므로

  int number1 = null;은 불가능하고 String name = null;은 가능합니다.

  -----------------------------------------------------------------------------

### 문자열 동등 비교

  [String Class API & Methods](https://baekjungho.github.io/java-api/)

  문자열 비교할때 알아둬야 할 2개의 문장이 있습니다. 첫 번째는 `메모리 번지가 같냐?`라는 질문이고
  두 번째는 `문자열이 같냐?`라는 질문입니다.

  - 메모리 번지가 같냐?

    ```java
      String str1 = "name"; // 메모리번지1
      String str2 = "name"; // 메모리번지1
      String str3 = new String("name"); // 메모리번지2

      // 문자열 비교가 아닌 메모리 번지 비교
      str1 == str2
      str2 == str3
    ```

  str1과 str2를 비교할때에는 `==`을 사용해도 상관없지만 str3와 비교할때는 `equals()`를 사용해야합니다.

  > String은 위에서 배운것 처럼 클래스타입이며, 객체입니다.
  >
  > str3은 새로운 객체를 생성(new 연산자), 따라서 새로운 메모리번지를 가집니다.

  단순히 메모리번지가 아닌 문자열 비교를 위해서는 String 클래스에서 제공하는 `equals()`메소드를 사용해야합니다.
  메소드 사용방법은 위에 나와 있습니다.

  - 문자열이 걑냐?

    equals()메소드는 결과를 true, false로 반환합니다.

    > 사용방법 : OriginalStringObject.equals(CompareStringObject);
    >
    > 원본.equals(비교);

    ```java
      String str1 = "name"; // 메모리번지1
      String str2 = "name"; // 메모리번지1
      String str3 = new String("name"); // 메모리번지2

      // 문자열 비교 메소드 equals()
      boolean result1 = str1.equals(str2);
      boolean result2 = str2.equals(str3);
    ```

### 문자열 크기 비교

  문자열중 어떤 것이 더 큰지(사전상 앞에 나오는지)를 비교하는 방법은 아래와 같습니다.

  - compareTo() 메소드 사용

  > if(a.compareTo(b) > 0) { System.out.println(“b가 사전상 앞에 있음, a가 더 큼”) }
  >
  > if(a.compareTo(b) < 0) { System.out.println("a가 사전상 앞에 있음, b가 더 큼")}

  compareTo() 메소드는 `a.compareTo(b)` 앞이 더 클 경우 숫자 1을 반환합니다.

  만약, 뒤의 숫자(b)가 더 클 경우 음수를 반환 합니다.

  [compareTo()를 사용한 문자열 정렬 문제](https://baekjungho.github.io/pro-2/)

  [Enum - compareTo()](https://baekjungho.github.io/java-enum/)

  -----------------------------------------------------------------------------

### 타입변환

  타입을 변환시키는 방법에는 2가지가 있습니다.

  `묵시적타입변환(자동, Promotion), 명시적타입변환(강제, Casting)`

  자동형변환은 작은타입이 큰타입으로 변환하는 것이고, 강제형변환은 큰타입 앞에 작은타입을 써줌으로써 작은타입으로 변환하는 것입니다.

  - Promotion Example

  ```java
    byte num1 = 8;
    int num2 = num1; // Promotion
  ```

  - Casting Example

  ```java
    long num1 = 800;
    int num2 = (int)num2;
  ```

### 상수

  상수는 파이(3.141592)와 같이 값이 고정적일때 사용합니다.

  또한, byte, int, long의 크기를 모를때 자바에서 지원해주는 최대값 상수와, 최소값 상수를 사용하면 편합니다.

  만약 상수를 사용하지않고 숫자로 적어준다면, 나중에 프로그램을 고칠때 여러군데를 고쳐야 할 것입니다.

  상수는 대문자로만 작성하며 단어가 겹칠경우 `_언더바`를 사용합니다.

  - 상수선언방법

  > 상수는 __static__ 을 붙여서 선언합니다.
  >
  > 상수 선언 키워드 : final

  __상수에는 static을 붙여서 선언하므로 클래스의 필드로 선언되야합니다. (메소드내 X)__

  ```java
    static final doulbe PI = 3.141592;
  ```

  **자바에서 제공하는 상수**  

  |종류    |최대값 상수||최소값 상수|
  |--------|-----------|------------|
  |byte| Byte.MAX_VALUE| Byte.MIN_VALUE|
  |short| Short.MAX_VALUE| Short.MIN_VALUE|
  |int| Integer.MAX_VALUE| Integer.MIN_VALUE|
  |long| Long.MAX_VALUE| Long.MIN_VALUE|
  |float| Float.MAX_VALUE| Float.MIN_VALUE|
  |double| Double.MAX_VALUE| Double.MIN_VALUE|

  단, boolean과 char타입은 자바에서 제공해주는 상수가 없다.

  최대값 상수와 최소값 상수를 이용하면, 데이터 타입 범위에 대해 검사할 수 있다.

### 삼항 연산자

  삼항 연산자 사용 방법은 아래와 같습니다.

  > 조건식 ? 값or식 : 값or식
  >
  > 조건식이 참이면 ?(물음표) 뒤 문장을 실행하고, 거짓이면 :(콜론) 문장을 실행합니다.

  ```java
  import java.util.Scanner;
  public class Example {
    public static void main(String[] args) {
      int score;
      Scanner sc = new Scanner(System.in);
      score = sc.nextInt();
      char grade = score > 90 ? 'A' :
                   score > 80 ? 'B' :
                   score > 70 ? 'C' :
                   score > 60 ? 'D' : 'F' ;
      System.out.println(score+"점은" + grade + "등급");

      sc.close();
    }
  }
  ```

### ArithmeticException, Infinity, NaN

   예외는 나중에 [예외(Exception)](https://baekjungho.github.io/java-exception/)파트에서 따로 배우겠지만 ArithmeticException은 연산과정에서 나올 수도 있는 예외 부분이라 먼저 알아두고 가면 좋습니다.

   > Division by zero -> ArithmeticException 발생 ex) 8 / 0 , 8 % 0
   >
   > Infinity -> ex) 8 / 0.0
   >
   > NaN -> ex) 8 % 0.0

   다음예제에서 예외처리를 위해 try-catch 구문을 사용했습니다.

   - ArithmeticException

   ```java
    public class ArithmeticExceptionExample {
     public static void main(String[] args) {
       int x = 8;
       int y = 0;

       try {
         int z = x % y; // 0으로 나머지연산을 할때 ArithmeticException 발생
         System.out.println("z: " + z);
       }catch(ArithmeticException e) {
         System.out.println("0으로 나누면 안됨");
       }
     }
    }
  ```

  - Infinity, NaN

  ```java
  public class InfinityNanExample {
    public static void main(String[] args) {
      double x = 8.0;
      double y = 0.0;

      double result1 = x / y;
      double result2 = x % y;

      if(Double.isInfinity(result1)) {
        System.out.println("Infinity");
      } else if(Double.isNaN(result2)) {
        System.out.println("NaN");
      }
    }
  }
  ```

  try-catch 구문은 try에서 문장을 실행하고 예외가 날 경우 catch에서 예외처리를 해준다고 생각하시면 됩니다.

## Scanner 클래스

  스캐너 클래스는 자바에서 사용자에게 입력을 받을 때 사용하는 클래스입니다.

  System.in : 저수준 스트림 객체 (타입을 스스로 변환해야함)

  Scanner : 고수준 스트림 객체 (타입을 자동으로 변환해줌)

  - Scanner 객체 생성방법

  > 맨위에 `import java.util.Scanner;`를 적어야 합니다.
  >
  > 객체생성 : Scanner sc = new Scanner(System.in);
  >
  > 스캐너를 사용하고 마지막에는 `sc(스캐너 객체).close()`로 닫아줘야 합니다.

  **Scanner Method**  

  |종류    |특징|
  |--------|----|
  |String next()|  문자열로 리턴합니다.|
  |byte nextByte()|  byte 타입으로 리턴|
  |short nextShort()| short 타입으로 리턴|
  |int nextInt()|  int 타입으로 리턴|
  |long nextLong()|  long 타입으로 리턴|
  |float nextFloat()|  float 타입으로 리턴|
  |double nextDouble()|  double 타입으로 리턴|
  |boolean nextBoolean()|  true or false로 리턴|
  |String nextLine()|  '\n'을 포함한 라인을 읽고 '\n'을 버린 나머지 리턴, 공백이 포함된 문자열 사용에 좋다.|
  |boolean hasNext()|  판단할 요소가 있으면 true / 아니면 false|

  Scanner 클래스 사용방법에 대해 잘 알고 있으면 나중에 배울 객체에 대해서 이해하기가 조금 쉽습니다.
  Scanner 클래스에서 제공하는 위 표에 나와있는 메소드들을 사용하기 위해서는 `객체명.메소드`로 접근하면 됩니다.

  ```java
  import java.util.Scanner;
  public class ScannerTest {
    public static void main(String[] args) {
      System.out.println("이름, 나이를 입력하시오 : ");
      Scanner sc = new Scanner(System.in); // 객체생성

      // 객체로 메소드에 접근 : 객체명.메소드()
      String name = sc.next(); // Scanner 클래스의 String next()
      System.out.println("이름 : ");
      int age = sc.nextInt(); // Scanner 클래스의 int nextInt()

      System.out.println("나이 : ");

      sc.close(); // 스트림 닫기
      // 스트림을 안 닫아줘도 실행에는 문제는 없지만
      // 경고(warning)가 발생한다.
    }
  }
  ```

  학점을 구하기 위해 점수를 입력 받을때 `int score = sc.nextInt();`의 방식보다는 아래와 같은 방식이 더 좋습니다.

  > int socre = Integer.parseInt(sc.nextLine());

### switch문

  switch문을 사용할때 필요한 것은 `switch, case, break, default`입니다.

  switch문의 구조는 다음과 같습니다.

  만약에 break를 작성하지 않는다면, 다음 case의 값과는 상관없이 해당 값부터 그 뒤에 case들이 모두 실행됩니다.

  ```java
  switch(변수){
  case "값1":
    실행문장;
    break;
  case "값n":
    실행문장;
    break;
  default:
    실행문장;
  }
  ```

## 반복문

  반복문의 종류에는 for문, while문, do-while문이 있습니다.

  또한 이 반복문 안에서 사용하는 키워드로 `break`와 `continue`가 있습니다.

### for문

  for문의 구조는 아래와 같습니다.

  > for(초기식; 조건식; 증감식){ 실행문장 }
  >
  > for(initialization; termination; increment) { statements }

  다음은 for문을 이용한 구구단 출력입니다.

  ```java
  public class GugudanExample {
	public static void main(String[] args) {
		for(int m=2; m<=9; m++) {
			System.out.println("----" + m + "단 -----");
			for(int n=1; n<=9; n++) {
				System.out.println(m + "x" + n + "=" + (m*n));
			   }
		  }
  	}
  }
  ```

### Enhanced For Loop

  Enhanced For Loop은 향상된 for문입니다. 배열에 있어서 사용합니다.

  구조는 아래와 같습니다.

  > for(declaration : expression) { statements }
  >
  > for(변수선언 : 배열이나 배열을 반환하는 함수) { 실행문장 }

  ```java
  String[] names = {"홍길동", "손오공", "사오정", "저팔계"};

  // Enhanced For Loop
  for(String name : names) {
    System.out.println(name);
  }

  // 출력 결과
  홍길동
  손오공
  사오정
  저팔계
  ```

  [제네릭 타입에 사용](https://baekjungho.github.io/java-generic/) 할 경우 다음과 같이 하면 됩니다.

  > for(Type Parameter Variable : 제네릭타입변수){ 실행코드 }

### while문

  while(조건식)문은 조건식이 참일때만 실행하는 반복문 입니다.

  while안의 조건식을 `true`로 주면 무한루프로 반복문안의 내용들이 계속 반복됩니다.

  `false`로 주게되면 반복문을 빠져 나오게 됩니다.

  ```java
    int i = 1;
    int sum = 0;
    while(i<=100){
      sum += i;
      i++;
    }
    System.out.println(sum);
  ```

  [반복문을 사용한 문제](https://baekjungho.github.io/java/problem-solving)

### do-while문

  do-while문은 do로 감싸진 문장을 먼저 실행하고, 그다음 while의 조건식을 판단합니다.

  ```java
    int i = 1;
    int sum = 0;
    do{
      sum+=i;
      i++;
    }while(i<=100);
  ```

### break와 continue

  break문과 continue문은 반복문 내에 사용하며, break는 반복문을 빠져나오고

  continue는 다시 반복문 꼭대기로 가서 실행하라는 문장입니다.

  ```java
    Scanner sc = new Scanner(System.in);
    String str = sc.nextLine();
    while(true){
      System.out.print("문자열을 입력하세요");
      if(str.equals("java")){
        System.out.println("문자열이 같습니다.");
        break;
      }
      else {
        System.out.println("문자열이 다릅니다.");
        continue;
      }
    }
  ```
