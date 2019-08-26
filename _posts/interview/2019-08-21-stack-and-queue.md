---
layout: post
title: Stack & Queue
tags: [dataStructure]
categories: [interview]
---

## Stack

### Concept

일반적으로 LIFO(Last In First Out)으로 불리며 한 쪽 끝에서만 자료를 넣고 뺄 수 있는 구조입니다.

### Operation

* pop() : stack에서 가장 위에 있는 항목을 제거합니다.
* push(item) : item 하나를 stack의 가장 윗부분에 추가합니다.
* top() : stack의 가장 위에 항목을 반환합니다.

### 사용 사례

재귀 Algorithm의 경우 Stack이 유용합니다.

* Recursive Algorithm - DFS 구현
* Web Browser History(뒤로가기) / 실행 취소(Undo)
* 역순 문자열 만들기
* 후위 표기법(postfix notation) 계산

## Queue

### Concept

일반적으로 FIFO(First In First Out)으로 불리며 자료의 입력과 출력을 각각 한 쪽 끝(front, rear)으로 제한한 자료구조입니다. 컴퓨터 Buffer에 주로 사용됩니다.

### Operation

* push(item) : item을 queue의 끝 부분에 추가합니다.
* pop() : queue의 첫번째 항목을 제거합니다.
* front() : queue 제일 앞에 있는 원소를 반환합니다.

queue가 꽉차서 더이상 데이터를 넣을 수 없는 경우를 Overflow라 하며, queue에 데이터가 없어서 꺼낼 수 없는 경우를 Underflow라 합니다.

### 사용 사례

데이터가 입력된 시간 순서대로 처리해야할 경우 유용합니다.

* BFS 구현 
* Process 관리 (Schedular)
* 대기 시간 처리

### 종류

크게 2가지로 Linear와 Circular가 있습니다.

Circular의 경우, 배열로 Queue를 구현시 Linear의 문제점을 보완하였습니다. queue의 생성과 삭제가 계속해서 발생하면 배열의 끝에 도달할 때, 데이터 공간이 남아있음에도 불구하고 Overflow가 발생합니다. (배열의 끝에 도달하였으므로)

이런 문제를 Circular를 통해 해결합니다. 또한 Linked List 로 구현된 경우 위와 같은 문제가 발생하지 않아서 굳이 Circular로 만들지 않아도 됩니다.

## Deque

양쪽 끝에서 자료를 넣고 뺄 수 있는 자료구조로 두 개의 포인터를 사용하여, 양쪽에서 삭제와 삽입을 발생 시킬 수 있습니다. 쉽게 Queue와 Stack을 합친 형태라고 생각할 수 있습니다.

## 출처

[자료구조 스택(Stack)이란](https://gmlwjd9405.github.io/2018/08/03/data-structure-stack.html)

[자료구조 큐(Queue)란](https://gmlwjd9405.github.io/2018/08/02/data-structure-queue.html)