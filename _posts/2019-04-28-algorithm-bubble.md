---
title: "버블 정렬(Bubble Sort)"
layout: post
category: Algorithm
tags: [BOJ, Programmers, Algorithm]
excerpt: "버블 정렬(Bubble Sort)에 대해 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

알고리즘 공부를 하면서 참고한 서적입니다.

_데이터 구조와 알고리즘_

## Bubble Sort

  버블 정렬(Bubble Sort)은 제일 간단하고 쉬운 정렬 알고리즘 입니다. `서로 인접한 두 원소`를 검사하여 정렬하는 알고리즘입니다.
  버블 정렬은 거품이 위로 올라가듯이 정렬을 실행한다고 해서 붙여진 이름입니다. __더 이상 교환이
  필요 없을때 까지 반복을 합니다.__ 일반적으로 삽입 정렬(Insertion Sort)이 버블 정렬(Bubble Sort)보다
  더 나은 성능을 보입니다.

  -----------------------------------------------------------------------------

### 알고리즘

  1~10번째의 인덱스에 저장된 값을 비교한다고 가정했을때 1번째 값과 2번째 값을 비교하여 더 큰 값을 위로 정렬시키고
  2번째 값과 3번째 값을 비교하여 더 큰 값을 위로 정렬 시키고 마지막-1번째 값과 마지막 값을 비교하여 정렬합니다.
  정렬 1회전을 수행하고나면 가장 큰 값은 맨 끝에 있게 되므로, 정렬에서 제외됩니다. 이렇게 정렬을 n회전 수행할 때마다
  정렬에서 n만큼의 값들이 제외됩니다.

  따라서 버퍼(buffer)와 반복문 2개가 필요합니다. n회전할 바깥 반복문, n회전에 대한 안쪽 반복문
  따라서 반복문이 2번 실시되므로 `O(n*n)` 만큼의 수행시간이 걸립니다.

  기본 버블정렬은 `O(n*n)`만큼의 수행시간이 걸리는데 여기에 하나의 플래그(flag)를 추가함으로써
  `O(n)`만큼의 수행시간을 낼 수 있습니다.

  > boolean 타입의 flag 하나 생성 (swapped)

  리스트가 이미 정렬이 완료 되었으면 `플래그(flag)`를 통해 나머지 단계를 생략 할 수 있습니다.

  -----------------------------------------------------------------------------

### 구현

  간단한 정수비교보다 문자열 정렬 예제를 통해 확인해 보겠습니다. Programmers 12917
  문제를 업그레이드 시킨 문제입니다.

  ```java
  public class StringDescOrderExample {

	public static void main(String[] args) {
		String[] alphabet = new String[] { "A", "B", "c", "D", "e", "v", "z", "k", "t", "R" };
		// 내림차순
		for(String str : sort(alphabet, true)) {
			System.out.print(str);
		}
		System.out.println();
		// 오름차순
		for(String str : sort(alphabet, false)) {
			System.out.print(str);
		}

	}
	public static String[] sort(String[] array, boolean descOrder) {
		String temp; // buffer
		boolean swapped = true; // flag
		if(descOrder) {
			for(int i=0; i<array.length-1 && swapped; i++) {
				swapped=false;
				for(int k=0; k<array.length-1-i; k++) {
					if(array[k].compareTo(array[k+1]) < 0) {
						temp = array[k];
						array[k] = array[k+1];
						array[k+1] = temp;
						swapped=true;
					}
				}
			}
			return array;
		} else {
			for(int i=0; i<array.length-1; i++) {
				for(int k=0; k<array.length-1-i; k++) {
					if(array[0].compareTo(array[k+1]) > 0) {
						temp = array[k];
						array[k] = array[k+1];
						array[k+1] = temp;
					}
				}
			}
			return array;
		}
	 }
  }
  ```

  [compareTo() 메소드](https://baekjungho.github.io/java-operator/#%EB%AC%B8%EC%9E%90%EC%97%B4-%ED%81%AC%EA%B8%B0-%EB%B9%84%EA%B5%90)는 String 클래스에서 제공하는 문자열 크기 비교에 사용되는 메소드 입니다.
