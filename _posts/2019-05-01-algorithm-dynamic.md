---
title: "동적계획법"
layout: post
category: Algorithm
tags: [BOJ, Programmers, Algorithm]
excerpt: "동적 계획법(Dynamic Programming)에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

알고리즘 공부를 하면서 참고한 서적입니다.

_데이터 구조와 알고리즘_

## Dynamic Programming

  동적 계획법(Dynamic Programming)은 복잡한 문제를 간단한 `여러 개의 문제(부속)`로 나누어 푸는 방법을 말합니다.
  즉, 큰 문제를 작은 문제들로 나눠서 해결하고 해결한 문제의 값을 저장하여(Memoization) 다음에 나왔을때
  사용하며, 해결한 문제들을 결합하여 최종 문제를 해결합니다. 동적 계획법의 특징은
  문제를 풀기위한 `순환식`을 세우고 이것을 계산하는 과정을 말합니다.

  Programming(계획법)의 의미는 코딩이 아니라 테이블(table)을 채운다는 의미입니다.

  > Dynamic Programming(DP) : Bottom-up, Memoization(Top-down), Binominal 방식을 사용
  >
  > Dynamic Programming(DP) : 순환식 세우는것이 가장 중요합니다.
  >
  > 순환식은 Memoization이나 Bottom-up 방식을 사용하여 풉니다.

  Bottom-up방식은 `재귀가 필요 없고 중복값 제거`효과를 지니고 있습니다.

  > Bottom-up : 기본값 위치에서 부터 시작

  - __Bottom-up__

    ```java
    public static int binomial(int n, int r){
      for(int i=0; i<=n; i++){
        for(int k=0; k<=r && k<=i; k++){
          if(n==r || r==0) binom[i][k] = 1;
          else binom[i][k] = binom[i-1][k-1] + binom[i-1][k];
        }
      }
      return binom[n][r];
    }
    ```

  - __동적 계획법 속성__

    - Optimal Substructure(최적 부속 구조)

      부속 문제들에 대한 최적의 해답을 가진 문제의 최적의 해답

    - Overlapping Subproblems(겹치는 부속 문제)

      여러번 반복되는 몇 가지 부속 문제들을 포함하는 재귀적 해법

  - __동적 계획법을 푸는 방법 두 가지__

    1. 상향식 동적 계획법 : Bottom-up
    2. 하향식 동적 계획법 : Top-down(Memoization)
