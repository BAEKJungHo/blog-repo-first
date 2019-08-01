---
title: "삽입 정렬(Insertion Sort)"
layout: post
category: Algorithm
tags: [BOJ, Programmers, Algorithm]
excerpt: "삽입 정렬(Insertion Sort)에 대해 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

알고리즘 공부를 하면서 참고한 서적입니다.

_데이터 구조와 알고리즘_

## Insertion Sort

  삽입 정렬(Insertion Sort)은 단순하고 효율적인 비교 정렬입니다. 구현이 간단하며, 작은 데이터에 효율적이고
  최악의 경우 O(n*n)으로서 버블 정렬과 선택 정렬과 같은 복잡도를 가지지만 실무에서 더 효육적입니다.

  > 적응적 : 입력 리스트가 선정렬되었을 경우 삽입 정렬은 d가 반전(inversion)의 횟수 일때 O(n+d)
  >
  > 제자리 : 일정한 크기의 부가 메모리 공간 O(1)만 있으면 된다.
  >
  > Online : 삽입 정렬은 입력 시스트를 받으면서 정렬 가능

  -----------------------------------------------------------------------------

### 알고리즘

  입력 항목이 더 이상 남아 있지 않을 때까지 매번 반복될 때마다 삽입 정렬은 입력 데이터로부터
  항목을 제거해서 이미 정렬된 리스트(배열)의 정확한 위치에 삽입합니다. 정렬은 보통 제자리에서
  일어납니다. 자료 배열의 모든 요소를 앞에서부터 차례대로 이미 정렬된 배열 부분과 비교 하여, 자신의 위치를 찾아 삽입함으로써 정렬을 완성하는 알고리즘입니다. 처음 Key값은 두 번째 자료부터 시작합니다.

  8, 10, 1, 5, 7, 2를 정렬한다고 가정할때 첫 번째 key는 10입니다. 10과 0번째 인덱스를 비교하여 정렬
  두 번째 key는 1이며 1, 2번째 인덱스와 비교하여 정렬 ..... 다음과 같은 반복을 수행합니다.

  최악의 경우는 key가 비교하는 다른 모두 보다 작을경우 O(i-1)의 시간이 걸립니다.
  평균의 경우는 O(i/2)의 시간이 걸립니다. 복잡도는 최악 최선 평균의 경우 O(n*n)입니다.

  -----------------------------------------------------------------------------

### 구현

  ```java
  public class InsertionSort {

	public static void main(String[] args) {
		int[] array = new int[]{10, 1, 9, 2, 8, 3, 7, 4, 6, 5};

		for(int srt : sort(array)) {
			System.out.print(srt);
		}
	}

	public static int[] sort(int[] array) {
		int key, i, k;
		for(i=1; i<array.length; i++) {
			key = array[i]; // key는 2번째 인덱스 부터 시작
			// 현재 정렬된 배열은 i-1까지이므로 i-1번째부터 역순으로 조사
			// key 값보다 정렬된 배열에 있는 값이 크면 k번째를 k+1번째로 이동
			for(k=i-1; k>=0 && array[k]>key; k--) {
				array[k+1] = array[k];
			}
			array[k+1] = key;
		}
		return array;
	 }
  }
  ```
