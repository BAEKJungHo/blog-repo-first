---
title: "이항계수(Binominal)"
layout: post
category: Algorithm
tags: [BOJ, Programmers, Algorithm]
excerpt: "이항계수(Binominal)에 대해 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

알고리즘 공부를 하면서 참고한 서적입니다.

_데이터 구조와 알고리즘_

## Binominal

  - __Binomial Algorithm__

    다음은 이항계수 알고리즘을 구하는 식입니다. 위 조합구하는 식과 비슷합니다.
    이항계수는 n개 중에 r개를 선택하는 경우의 수를 구할 때 사용합니다.
    n == r이거나 r이 0일때 1을 반환하며, 아래 처럼 재귀 함수를 반복하다보면
    나중에 n이 r과 같아지거나 r이 0이 되는 `base case`에 도달하게 되며, 값을 찾게 됩니다.
    Binominal Algorithm보다 더 효과적이고 빠른 계산이 가능한 방식이 Memoization입니다.

    ```java
    public static int binomial(int n, int r){
        if(n==r || r==0) return 1;
        else return binominal(n-1, r-1) + binominal(n-1, r);
    }
    ```
