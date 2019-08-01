---
title: "선택 정렬(Selection Sort)"
layout: post
category: Algorithm
tags: [BOJ, Programmers, Algorithm]
excerpt: "선택 정렬(Selection Sort)에 대해 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

알고리즘 공부를 하면서 참고한 서적입니다.

_데이터 구조와 알고리즘_

## Selection Sort

  선택 정렬(Selection Sort)은 `제자리(in-place)` 정렬 알고리즘 입니다.
  작은 파일들에서 잘 동작합니다. 구현이 쉽고, 부가적인 저장공간(다른 추가 메모리)이 필요 없습니다.
  값이 크고, 키가 작은 항목들에 대해 적합합니다.

  -----------------------------------------------------------------------------

### 알고리즘

  1. 리스트(배열)에서 최솟값을 찾는다.
  2. 이 값을 맨 앞 인덱스의 값과 교환한다.
  3. 교환된 첫 번째 인덱스를 제외한 나머지 인덱스에 대해 과정을 실시한다.
  4. 전체 리스트(배열)이 정렬될 때 까지 이 과정을 모든 항목에 대해 반복한다.

  1회전을 수행하고 나면 최솟값이 맨앞에 위치하게 됩니다. 마지막 숫자는 자동정렬 되기 때문에 n-1만큼 반복하면 됩니다.
  성능은 최악의 경우 O(n*n), 최선의 경우 O(n), 평균의 경우 O(n*n)으로서 버블정렬과 같습니다.

  최솟값을 찾아서 선택정렬을 할 경우 `오름차순(osc)`으로 정렬됩니다. 이것을 역이용해서
  최댓값을 찾아서 선택정렬을 할 경우 `내림차순(desc)`로 구현할 수도 있습니다.

  -----------------------------------------------------------------------------

### 구현

  선택정렬을 이용해서 오름차순과 내림차순 정렬 구현하기

  ```java
  public class SelectionSort {

  	public static void main(String[] args) {
  		int[] array = new int[]{10, 1, 9, 2, 8, 3, 7, 4, 6, 5};
  		// 최솟값을 찾아서 오름차순 정렬
  		for(int srt : sort(array, true)) {
  			System.out.print(srt);
  		}
  		System.out.println();
  		// 최댓값을 찾아서 내림차순 정렬
  		for(int srt : sort(array, false)) {
  			System.out.print(srt);
  		}
  	}

  	// 마지막 숫자는 자동정렬 되기 때문에 n-1만큼 반복
  	public static int[] sort(int[] array, boolean descOrder) {
  		int min, max, temp;
  		if(descOrder) { // 내림차순
  			for(int i=0; i<array.length-1; i++) {
  				max = i;
  				// 최댓값 찾기
  				for(int k=i+1; k<array.length; k++) {
  					if(array[k] > array[max]) {
  						max = k;
  					}
  				}
  				// 항목 교환
  				temp = array[max];
  				array[max] = array[i];
  				array[i] = temp;
  			}
  			return array;
  		} else { // 오름차순
  			for(int i=0; i<array.length-1; i++) {
  				min = i;
  				// 최솟값 찾기
  				for(int k=i+1; k<array.length; k++) {
  					if(array[k] < array[min]) {
  						min = k;
  					}
  				}
  				// 항목 교환
  				temp = array[min];
  				array[min] = array[i];
  				array[i] = temp;
  			}
  			return array;
  		}
  	}
  }
  ```
