---
title: "재귀(Recursive)"
layout: post
category: Algorithm
tags: [BOJ, Programmers, Algorithm]
excerpt: "재귀(Recursive) 함수에 대해 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

알고리즘 공부를 하면서 참고한 서적입니다.

_데이터 구조와 알고리즘_

## Recursive

  재귀함수(Recursive Function)는 자신의 메소드(함수)에서 자기자신을 호출 하는 것을 말합니다.
  재귀적 방법은 자신의 복사본을 호출하여 더 작은 문제를 풀게함으로써 문제를 해결합니다.
  이를 재귀적 단계라고합니다. 함수가 재귀를 호출하지 않는 것을 `기본경우(base case)`라고 합니다.

  예를들어 재귀를 사용하는 팩토리얼의 정의는 다음과 같습니다.

  - n=0일때 n! = 1
  - n>0일때 n! = n * (n-1)!

  - __기본경우(base case)와 재귀(recursive)__

    ```java
    int Fact(int n) {
      // 기본경우(base case)
      if(n==1) { return 1; }
      else if(n==0) { return 1; }
      // 재귀(recursive)
      else
        return n * Fact(n-1);
    }
    ```

    __모든 재귀 함수는 기본경우(base case)에 종료해야 합니다.__

  - 특징

    1. 무한반복되는 재귀호출은 사용하면 안됨.
    2. 코드가 간결해지고 오류 수정이 쉽다.
    3. 기억공간, 메모리(스택 프레임)이 많이 필요하다.
    4. 같은 값에 대해 중복되는 코드가 발생한다.

  - 사용되는 예

    피보나치 수열, 팩토리얼, 병합 정렬, 퀵 정렬, 이진검색, 그래프 탐색, 깊이 우선 탐색,
    분할 정복 알고리즘, 동적 계획법의 예, 백트래킹 알고리즘 등

    ```java
    public class RecursiveExample {
      int i = 0;

      public static void main(String[] args) {
          recursive(10);
      }

      public static void recursive(int i) {
          int sum = 0;
          System.out.println();
          if(i < 0) {
              System.out.println(sum);
              return;
          } else{
              sum += i;
              i--;
              recursive(i); // 재귀함수 호출
          }
      }
    }
    ```
