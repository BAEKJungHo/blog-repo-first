---
title: "Javascript Basic"
layout: post
category: Javascript
tags: [Javascript, HTML5, CSS3]
excerpt: "자바스크립트 작성방법, 변수, 식별자, 함수, 배열 등에 대해 알아봅시다."
author: BAEKJungHo
---

* content
{:toc}

HTML5는 웹의 구조를 CSS3는 웹 페이지를 꾸며주고, 자바스크립트는 웹 페이지를 동작하게 한다.

## Javascript Basic1

  ![run](/images/posts/201903/run.jpg)

## 자바스크립트 작성방법

  자바스크립트 작성방법은 `<script>자바스크립트 실행문장</script>`, `<head> or <body>내부에 작성`, `[ 파일명.js ]`으로 저장해서 <head><title><script src="파일명.js></script></title></head>으로 HTML문서에 삽입 가능합니다.

  ```
  <!DOCTYPE html>
  <html lang="ko">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>자바스크립트 작성 및 삽입방법</title>
    <script src="javascripts.js">
          // 자바 스크립트 코드 작성
    </script>
  </head>
  <body>
    <h3>자바 스크립트 배우기</h3>
    <hr>
    <script>
      // 자바 스크립트 코드 작성
    </script>
  </body>
  </html>
  ```

## 자바스크립트 주석문

  `// 한줄 주석문`, `/* 여러줄의 주석문 */`

## 자바스크립트 내에서 HTML5 코드 작성

  자바언어에 메시지를 출력하기위한 수단으로 `System.out.print() / System.out.println() : 줄바꿈이 있다` 자바 스크립트에서는 `document.write()`이라는 코드를 이용한다. document.writeln()은 줄바꿈이 아니라 단순히 한칸 띄어쓰는 효과이다.
  자바스크립트 언어에서 줄바꿈을 하기 위해서는 document.write("br")을 사용하여 줄바꿈을 한다. br은 <>방식으로 표시해줘야한다.
  `쌍따옴표 안에 HTML 태그를 넣어 사용`할 수 있다.
  즉, document.write()는 웹 페이지에 화면을 출력하기 위해서도 사용되지만, HTML코드를 삽입하여 적용시키기 위해서도 사용된다.

  ```
  <script>
    document.write("자바의 System.out.print와 같습니다");
    document.write("<br>"); // 자바의 Sytem.out.println과 같습니다.
    document.writeln(); // 띄어쓰기 한칸 효과입니다.
    document.wrtie("&nbsp"); // 띄어쓰기 한칸 효과입니다.
    document.write(" ") // 띄어쓰기 효과가 적용되지 않습니다.
  </script>
  ```

## 자바스크립트 다이얼로그

  다이얼로그에는 `prompt(), confirm(), alert()`가 있다.

  ```
  <script>
    var num = parseInt(prompt("숫자를 입력하세요", "8")); // 숫자를 입력받는 다이얼로그 출력
    // prompt("내용", "디폴트 값")
    // 초기에 디폴트값을 비워두면 입력창에 아무것도 적히지 않은 상태로 다이얼로그가 출력된다.

    var message = confirm("메시지를 전송 할까요?"); // 확인, 취소버튼이 있는 다이얼로그 출력
    // User가 확인을 누르면 true 리턴
    // User가 취소 or 다이얼로그를 닫으면 false 리턴

    alert("경고 및 알림창, 단순 메시지를 띄워준다"); // 쉽게 경고, 알림창이라 생각하면 된다.
  </script>
  ```


## 변수와 식별자

  자바스크립트에서의 변수와 식별자에 대해 알아봅시다.

### 변수 선언 방법

  자바스크립트에서 변수를 선언하기 위해 `[ var이라는 키워드 ]`를 사용한다. 어떤 언어로 프로그래밍을 하던지 변수선언이 중요하다. 왜냐하면 변수를 잘 선언해야 나중에 봐도 어떤 역할을 하는지 알 수 있다. 또한 프로그램을 만든 사람 뿐만 아니라 이것을 수정할 다른 사람들 혹은 자신의 후임들 등 누구든 이 코드를 보고서 식별자명을 보기만해도 무슨 역할을 하는지 대충 감이 오게 작성하는게 중요하다.
  예를들어 남자와, 여자를 나타내는 식별자를 작성하려고하는데 어떤 사람은 대충 var a; var b; 이렇게 작성 할 것이다. 위 처럼 작성해도 혼자만 보는것이면 상관없지만, 애초에 습관을 잘 들여서 var male, var female 이런식으로 작성하는게 누가봐도 무슨 역할을 하는지 알 수 있기 때문에 좋다.
  `[ 변수 = 기억장소 ] [ 자바스크립트에서의 변수는 데이터 타입을 따로 지정하지 않는다 ]` 문자열이든, 정수든, 실수든 var이라는 키워드를 사용해서 값만 넣어주면된다.
  또한 자바에서는 쌍따옴표("")는 문자열을 나타내고 따옴표('')는 문자를 나타냈지만, `자바스크립트에서는 둘다 문자열`을 나타낸다.

### 지역변수와 전역변수

  전역 변수 : 함수 밖에서 선언되거나 함수 내에서 var 키워드 없이 선언, 프로그램 전역에서 사용 가능하다.
  지역 변수 : 함수 안에서 var 키워드로 선언. 선언된 함수 내에서만 사용 가능하다.
  전역변수에 접근할때는 this 키워드를 사용한다.

  ```
  <script>
  var global; // 전역변수
  fucntion sum(num1, num2) {
      var local = 10; // 지역변수
      num_global = 10; // 전역변수
      this.x = 100; // 전역변수에 접근하여 100을
  }
  </script>
  ```

### 식별자와 문장

  자바스크립트에도 식별자 규칙이 있다. 자바와 동일하다.

  - 대, 소문자를 구별

  - 예약어(if, for 등) 사용불가

  - 첫 단어가 숫자로 시작할 수 없음

  - % 사용 불가 ( _ , $는 사용 가능)

  문장(Statement)을 구별하는 방법은 ;(세미콜론)으로 구분한다. 한줄에 한문장만 있는 경우에는 생략 할 수는 있지만, 가급적이면 세미콜론을 사용하는 습관을 들이는게 좋다.

## 함수(function)

  함수를 사용하는 이유는 중복되는 작업(ex) 반복문 등)을 함수라는 것을 통해서 선언하고 동작하게 만들어준다음, 본문 <body>안에서는 그 함수를 호출하기만 하면 사용할 수 있기 때문입니다. 예를 들어 자바의 객체, 생성자, 메소드 등을 사용하는 이유는 프로그래머가 만들고자 하는 어떤 사물 등에 대한 공통 속성을 뽑아서 메소드에 정의하고 메소드를 호출만 하면 사용 할 수 있게 만들어주는것입니다.

### 함수를 사용하는 예제

  ```
    [함수를 선언한 코드]

    <!DOCTYPE html>
    <html lang="ko">
    <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <meta http-equiv="X-UA-Compatible" content="ie=edge">
      <title>함수</title>
      <script>
        function sum(init, final, inc){
          var total = 0;
          for(var i=init; i<=final; i+=inc)
            total += i;
          return total;
        }
      </script>
    </head>
    <body>
      <h3>함수 adder()</h3>
      <hr>
      <script>
        sum0 = sum(1, 100, 1);
        sum1 = sum(1, 100, 2);
        sum2 = sum(2, 100, 2);
        sum3 = sum(3, 100, 3);
        sum4 = sum(4, 100, 4);
        sum5 = sum(5, 100, 5);

        document.write("1~100까지의 합 : " + sum0 + "<br>");
        document.write("1~100까지의 홀수 합 : " + sum1 + "<br>");
        document.write("1~100까지의 짝수 합 : " + sum2 + "<br>");
        document.write("3의 배수의 합 : " + sum3 + "<br>");
        document.write("4의 배수의 합 : " + sum4 + "<br>");
        document.write("5의 배수의 합 : " + sum5);
      </script>
    </body>
    </html>



    [함수를 선언 하지 않고 반복문으로만 사용한 코드]

    <script>
        for(i=1; i<=100; i++) {} // 1~100까지 합에 대한 반복문
        for(i=1; i<=100; i+=2) {} // 1~100까지 홀수의 합에 대한 반복문
        for(i=2; i<=100; i+=2) {} // 1~100까지 짝수의 합에 대한 반복문
        for(i=3; i<=100; i+=3) {} // 1~100까지 3의 배수의 합에 대한 반복문
        for(i=4; i<=100; i+=4) {} // 1~100까지 4의 배수의 합에 대한 반복문
        for(i=5; i<=100; i+=5) {} // 1~100까지 5의 배수의 합에 대한 반복문
    </script>
  ```

### 함수 선언 및 호출방법

  선언방법에는 2가지가있습니다. `선언적 함수, 익명함수`가 있습니다.

  아래의 방법 1로 함수를 선언하면, 함수 호출을 선언 전과, 후 둘다 가능하게 할 수 있습니다. 단, 방법 2로 선언한 함수는 호출을 무조건 뒤에 해야합니다.

  ```
    <script>
      // 함수선언 방법 1
      functionName(value1, value2);
      function functionName(arg1, arg2) {
          // code
      }
      // 함수선언 방법 2
      var function_Name = function(arg1, arg2) {
          // code
      }
      // 함수호출
      // value에 값을 넣어서 매개변수에 전달한다.
      functionName(value3, value4);
      function_Name(value5, value6);
    </script>
  ```

### 함수의 구조

  ```
    <script>
        function 함수(매개변수1, 매개변수2 ...) {
            var num1 = 0, num2 = 100;
            var sum;
            sum = num1 + num2;
            return sum; // 반환 키워드 : return, 반환 값 : sum
        }
    </script>
  ```

### 함수의 종류

  함수의 종류에는 eval(), parseInt(), isNaN(), parseFloat(), isFinite(), Number(), String()등이 있습니다.

#### isNaN()

  - isNaN(value) : value값이 숫자이면 false, 숫자가 아니면 true 리턴

  ```
    <script>
      var num = prompt("숫자 입력", "0");
      // num이 숫자이면 if문을 실행
      if(!isNaN(num)) {
          var n = parseInt(num);
      }
    </script>
  ```

#### parseInt()

- parseInt(value) : value값을 정수형으로 변환

  value 값이 017, 015 등 앞자리가 0으로 시작하면 8진수를 10진수화 시키고

  value 값이 0x56, 0x89 등 앞 두자리가 0x로 시작하면 16진수를 10진수화 시킵니다.

  ```
    <script>
        // 변수 num을 선언하고 숫자를 입력받고 입력받은
        // 숫자를 정수화 시켜서 변수 num에
        var num = parseInt(prompt("숫자 입력", "0"));
    </script>
  ```

### 내부함수

  함수 안에 또다른 함수가 있는 것을 말합니다. 함수의 외부에서 내부 함수를 호출 할 수 없습니다.

  ```
    function functionName(arg1, arg2) {
      function function_Name(arg3) {
          return arg3;
      }
      return arg1 * arg2;
    }
  ```

## 배열(Array)

  배열은 여러개의 데이터를 한꺼번에 저장할 수 있습니다. 배열은 Array객체이고, 따라서 Array 객체의 메소드를 사용할 수 있습니다.

### 배열 생성방법

  배열을 생성할 때는 new 키워드를 사용하거, []를 사용합니다. 배열의 크기 : `배열이름.length`

  ```
    <script>
      var people = ["james", "john", "mike"] // 배열의 크기 3
      var cities = new Array(); // new연산자를 사용하여 비어있는 배열 생성

      // 배열이름.length은 배열의 크기를 나타낸다.
      for(i=0; i<people.length; i++) {
          cities[i] = people[i]; // 배열 복사
      }
      document.write(cities[0]); // james 출력
    </script>
  ```

  > 배열은 new 연산자를 사용하여 만들 수도 있고, []를 사용하여 안에 값을 넣은상태(초기화)로 만들 수 도 있습니다.
  >
  > 위 처럼 빈 배열을 생성하면( var cities = new Array() ) 데이터를 넣을 때마다 자동으로 크기가 늘어납니다.

### Array 객체의 메소드

  여러가지 메소들이 있지만 2가지를 소개하고자 합니다. sort()와, splice()입니다.

  ```
    <script>
      var names = ["James", "John", "Mike"];
      splice(1,1); // 1번째 인덱스부터 1개 삭제 - John
      splice(0.2); // 0번째 인덱스부터 2개 삭제 - James, John
    </script>
  ```

  sort() 메소드는 문자열을 사전순으로 정렬하고 이를 복사한 새 배열을 리턴합니다. 이 메소드의 장점은, 배열 내의 데이터를 비교하고 값을 크기순으로 정렬하거나 할 때 "버블정렬"등을 사용하지 않고도 한 줄 코드로 간단하게 사용할 수 있습니다.

## DOM트리, BOM트리

  BOM 객체는 HTML 태그나 페이지 내용과 관계가 없고, 오직 브라우저에 관한 객체들의 집합입니다.

  DOM 객체는 HTML 태그나 페이지의 내용을 변경하기 위해 모인 객체들의 집합입니다.

  (브라우저마다 BOM 객체가 다르고, BOM에 대한 W3C 표준이 없다.)

  ![tree](/images/posts/201903/tree.jpg)

## Event와 EventListener

  이벤트(Event) : 마우스클릭, 휠 클릭, 키보드 입력등의 물리적 효과

  이벤트 객체(Event Object) : 해당 이벤트에 대한 정보, 마우스 좌표 등을 이벤트 리스너에게 전달한다.

  이벤트 리스너(Event Listener) : 물리적인 효과가 왔을때 실행할 자바스크립트 코드

  ☞ 브라우저에서 발생하는 모든 변화들이 이벤트이다.

### Operation process

  ![event](/images/posts/201903/event.jpg)

  먼저 브라우저에서 이벤트가 발생하면 이벤트 객체라는 것이 생성된다. 이 이벤트 객체는 이벤트 리스너에게 전달되어서 이벤트 리스너가 자바스크립트코드로 (onclick, onmouseover 등) 이벤트를 처리하고 그 결과를 브라우저에 반영합니다.

  예를들어, 우리가 네이버에서 검색내용을 입력하고 검색을 누르면 그 결과가 나오듯이, 이벤트리스너를 잘 활용하면 그 결과를 얻을 수 있습니다.


### DOM 객체를 찾는 방법

  - 태그이름

    ```
    <script>
      var p = document.getElementsByTagName("p");
    </script>

    <p>DOM 객체를 찾는 방법</p>
    ```

  - class 속성

    ```
    <script>
      var receive = document.getElementsByClassName("receive");
      var len = receive.length; // receive 클래스 속성을 가진 태그 의 수가 2개
    </script>

    <p class="send">메시지 보내기</p>
    <p class="receive">메시지 받기1</p>
    <p class="receive">메시지 받기2</p>
    ```

  - name 속성

    ```
    <script>
      var check = document.getElementsByName("check")
      var len = check.length; //check name 속성을 가진 태그 의 수가 2개
    </script>

    <input type='checkbox' name='check' value='값1'/> 값1<br/>
    <input type='checkbox' name='check' value='값2'/> 값2<br/>
    ```

  - id 속성

    ```
    <script>
      var p = document.getElementById("p1");
    </script>

    <p id="p1>DOM 객체를 찾는 </p>
    ```

### EventListener 작성방법

  이벤트 리스너 작성방법에는 총 4가지가 있습니다.

  - HTML 태그 내에 작성

    ```
    <p onclick="this.style.backgroundColor="'yellowgreen'"></p>
    ```

  - 이벤트 리스너에 속성 등록

    ```
    <script>
      function click(){
      // 내용
      }
      // DOM 객체
      var p = document.getElementById("p1");
      p.onclick = click;
    </script>

    <p id="p1"></p>
    ```

  - addEventListener() 사용

    ```
    <script>
      function start(){
      // DOM 객체 찾기
        var p = document.getElementById("p1");
        p.addEventListener("click", click);
        p.addEventListener("click", over);
      }
      function click() {
        // 내용
      }
      function over() {
        // 내용
      }
    </script>

    <p id="p1"></p>
    ```

  - 익명함수 사용

    ```
    <script>
      var p;
      function start() {
        p = document.getElementById("p1");
        p.onclick = function () {
            this.style.backgroundColor="red";
        }
      }
    </script>

    <p id="p1"></p>
    ```
