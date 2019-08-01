---
title: "백트래킹(BackTracking)"
layout: post
category: Algorithm
tags: [BOJ, Programmers, Algorithm]
excerpt: "백트래킹(BackTracking)에 대해 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

알고리즘 공부를 하면서 참고한 서적입니다.

_데이터 구조와 알고리즘_

## BackTracking

  백트래킹(BackTracking)은 역추적, 퇴각검색이라 불립니다. 백트래킹은 분할 정복을 이용한
  `완전 검색(Exhaustive Search)` 기법입니다.

  한정 조건을 가진 문제를 풀려는 전략이며, 모든 조합을 시도하여 해를 구합니다.
  해를 얻을 때 까지 모든 가능성을 시도하며, 모든 가능성은 하나의 트리처럼 구성 가능합니다.
  그리고 가지중에 해결책이 있습니다.

  - 사용되는 예

    이진 문자열, k-ary 문자열, 배낭 채우기(Knapsack Problem), 문자열 일반화(Generalized Strings),
    선교사와 식인종, 해밀턴 사이클 등
