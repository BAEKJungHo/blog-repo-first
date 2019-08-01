---
title: "Memoization"
layout: post
category: Algorithm
tags: [BOJ, Programmers, Algorithm]
excerpt: "Memoization에 대해 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

알고리즘 공부를 하면서 참고한 서적입니다.

_데이터 구조와 알고리즘_

## Memoization

  Memoization은 프로그래밍을 할 때 반복되는 결과를 메모리에 저장해서 다음에 같은 결과가 나올 때 빨리 실행하는 코딩 기법을 말합니다.
  예를들어 배열을 사용하여 Memoization기법을 사용하면 한번 계산한 값을 배열에 저장하고 나중에 필요할 때 꺼내서 사용하면 됩니다. 주로 동적계획법에서 많이 사용합니다. Memoization은 `Top-down` 방식이며 실제 필요한 `sub problem`을 해결합니다.

  __즉, Memoization은 중복되는 연산을 없애기 위해서 만들어진 기법입니다__

  > nCr(조합) : n개의 숫자에서 r개를 뽑는 경우의 수
  >
  > 공식 : nCr = n-1Cr-1 + n-1Cr

  - __EXAMPLE__

    ```java
    import java.util.Scanner;

    public class MemoizationExample {
      static int[][] combArray;
      public static void main(String[] args) {
          Scanner sc = new Scanner(System.in);

          combArray = new int[18][18];

          int n = Integer.parseInt(sc.nextLine());
          int r = Integer.parseInt(sc.nextLine());
          System.out.println(combination(n,r));
      }

      // nCr = n-1Cr-1 + n-1Cr
      // Memoization Algorithm
      public static int combination(int n, int r){
          if(n==r || r==0) return 1;
          else if(combArray[n][r] > 0) return combArray[n][r]; // 배열에 값이 저장되어있으면 가져와서 사용
          else {
            // Recursive
            combArray[n][r] = combination(n-1, r-1) + combination(n-1, r);
            return combArray[n][r];
        }
      }
     }
     ```

     Memoization Algorithm 위 코드는 배열이 0으로 초기화 되어있다고 가정하고 값이 0일때 계산을 진행하고
     아닐 경우 인덱스의 배열 값을 반환합니다.

  - __Cache & Caching__

     만약 위 코드에서 다음과 같이 되어있다고 하면 사실상 위 문제를 해결하기 위해서는 아래처럼 캐싱하면 안됩니다.
     캐시와 캐싱에 대해 이해를 돕고자 적었습니다.

     ```java
     if(combArray[n][r]>0) {
       combArray[n][r] = combArray[n-1][r-1]; // Caching
       return combArray[n][r]; // Cache
     }
     ```

     위 처럼 중간에 저장하는 값을 `캐싱`이라고 하며 저장되는 배열을 `캐시`라고 합니다.
