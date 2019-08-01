---
title: "스택(Stack)"
layout: post
category: Algorithm
tags: [BOJ, Programmers, Algorithm]
excerpt: "스택(Stack)에 대해 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

알고리즘 공부를 하면서 참고한 서적입니다.

_데이터 구조와 알고리즘_

## Stack

  스택(Stack)은 데이터를 저장하기 위해 사용되는 자료구조 입니다.
  스택은 `LIFO(Last In First Out)` 후입선출, 나중에 들어간 데이터가 제일 먼저 빠져나오는
  구조입니다. 즉, 삽입과 삭제가 한쪽 끝에서 이루어지는 순서가 매겨진 리스트입니다.
  스택은 `물컵` 이라고 생각하시면 쉽습니다.

  > EX) 물컵안에 동전을 넣고 다시 넣은 동전을 빼내는 과정

  자바에서는 [Stack Collection](https://baekjungho.github.io/java-collection/#stack)이 있어서 따로 구현하지 않아도 push() 넣기, pop() 꺼내기등 필요한 메소드를 지원합니다.

  `언더플로우(Underflow)`는 스택이 비어있는 상태에서 데이터를 pop()하려는 것이고 `오버플로우(Overflow)`는 스택이 꽉 차있는 상태에서 데이터를 push()하려는 것입니다. 이 두 가지의 경우에는 예외(Exception) 처리를 해주어야 합니다.

  - __연산__

  1. push(data) : 데이터를 스택에 넣는다.
  2. int pop() : 스택에 제일 마지막에 추가된 항목을 삭제하고 리턴한다.
  3. int top() : 스택에 마지막에 추가된 항목을 삭제하지 않고 리턴한다.
  4. int size() : 스택에 저장된 항목의 개수를 리턴한다.
  5. isEmptyStack() : 스택에 항목이 저장되어 있는지 아닌지를 확인한다.
  6. isFullStack() : 스택이 가득 찼는지 아닌지를 확인한다.

  > 스택 적용 예 : 괄호 짝 맞추기, 인픽스(Infix)를 포스트픽스(Postfix)로 변환하기, 포스트픽스 수식 계산하기
  > 함수 호출 구현하기, 스팬(span) 찾기, HTML과 XML에서 태그(Tag) 짝 맞추기 등

  스택을 구현하는 방법에는 대표적으로 `배열, 동적 배열, 연결리스트`로 구현합니다.

  - __배열로 구현한 스택__

  ```java
  public class ArrayStack {
    private int top;
    private int capacity;
    private int[] array;

    public ArrayStack() {
      capacity = 1;
      array = new int[capacity];
      top = -1; // 스택이 비어 있는 경우
    }

    public boolean isEmpty() {
      return (top == -1);
    }

    public boolean isStackFull() {
      return (top == capacity -1);
    }

    public void push(int data) {
      if(isStackFull())
        System.out.println("Stack Overflow");
      else
        array[++top] = data; // top을 1씩 증가 시키고 top 위치에 데이터 저장
    }

    public int pop() {
      if(isEmpty()) {
          System.out.println("Stack is Empty");
          return 0;
      } esle return array[top--];
    }

    public void deleteStack() {
      top = -1;
    }
  }
  ```

  - __동적 배열로 구현한 스택__

  ```java
  public class DynamicArrayStack {
    private int top;
    private int capactiy;
    private int[] array;

    public DynamicArrayStack() {
        capacity = 1;
        array = new int[capacity];
        top = -1;
    }

    public boolean isEmpty() {
      return (top == -1);
    }

    public boolean isStackFull() {
      return (top == capacity -1);
    }

    public void push(int data) {
      if(isStackFull()) doubleStack();
      array[++top] = data;
    }

    private void doubleStack() {
      int newArray[] = new int[capacity * 2];
      System.arraycopy(array, 0, newArray, 0, capacity);
      capacity = capacity * 2;
      array = newArray;
    }

    public int pop() {
      if(isEmpty()) System.out.println("Stack Overflow");
      else return array[top--];
    }

    public void deleteStack() {
      top = -1;
    }
  }
  ```

  - __연결리스트로 구현한 스택__

  ```java
  public class LLStack extends Stack {
    private LLNode headNode;
    public LLStack() {
      this.headNode = new LLNode(null);
    }

    public void push(int data) {
      if(headNode == null) {
        headNode = new LLNode(data);
      } else if(headNode.getData() == null) {
        headNode.setData(data);
      } else {
        LLNode llNode = new LLNode(data);
        llNode.setNext(headNode);
        headNode = llNode;
      }
    }

    public int top() {
      if(headNode == null) return null;
      else return headNode.getData();
    }

    public int pop() {
      if(headNode == null) {
        throw new EmptyStackException("Stack Empty");
      } else {
        int data = headNode.getData();
        headNode = headNode.getNext();
        return data;
      }
    }

    public boolean isEmpty() {
      if(headNode == null) return true;
      else return false;
    }

    public void deleteStack() {
      headNode = null;
    }
  }
  ```

  - __배열과 연결리스트 구현의 차이__

    - 배열 : 연산 수행에 일정한 시간이 걸리며, 가끔 두배 확장 연산을 수행
    - 연결리스트 : 모든 연산에 일정한 시간 O(1)이 걸린다.
