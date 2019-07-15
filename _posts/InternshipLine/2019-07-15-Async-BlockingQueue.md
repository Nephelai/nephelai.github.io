---
layout: post
title: Asynchronous BlockingQueue in Java
tags: [OS]
categories: [InternshipLine]
---

Java에서 Thread를 기반으로 Async 동작을 구현하면 Blocking Queue를 보게 된다.

Blocking Queue란, Thread가 지금 자원을 할당받지 못해서 대기하는 Queue라는 자료구조이다. 여기서 Blocking이란 Queue가 꽉 찼을 때 Insert 시도 / Queue가 비었을 때 Evict을 막는다는 것이다. 즉, BlockingQueue의 구현체는 모두 Thread-safe이다.

## Blocking Queue의 종류와 특징

### ArrayBlockingQueue

고정된 배열에 일반적인 Queue를 구현한 Class, 생성 후 크기 변경이 불가합니다.

선택적으로 fairness policy를 두어 block한 thread들의 순차적 대기열을 생성합니다. 

### LinkedBlockingQueue

선택적으로 Bound가 가능한 LinkedList로 구현한 Queue, Capacity를 초기에 정해주지 않은 경우 MAX_VALUE 이용합니다.

용량을 초과하지 않는 한에서 node는 동적으로 삽입하게 됩니다.

Linked Queue의 경우 일반적으로 배열기반 Queue보다 동시성이 높은 App에서 높은 처리량(Throughput)을 갖게 됩니다.

### PriorityBlockingQueue

PriorityQueue 같은 정렬 방식을 지닌 용량 제한이 없는 Queue입니다. 입력 무제한(unbounded)으로 설계되어서 Insert 작업 중 fail이 나는 경우는 OutOfMemory Error 뿐입니다.

### SynchronousQueue

Queue 내부로의 Insert 작업이 다른 Thread의 remove 작업과 반드시 동시에 일어나야 합니다. (서로 대칭되는 작업이 생길 때까지 wait)

이 Queue의 경우 내부 Capacity를 지니지 않습니다. Insert와 Evict이 동시에 발생하기 때문입니다.



### Blocking Concurrency Algorithm

Thread가 BlockingQueue에 Insert를 하려고 하지만 Queue에 공간이 없으면 Thread는 공간이 생길 때까지 대기합니다. 

### Non-Blocking Concurrency Algorithm

NonBlocking 자료구조도 존재합니다. 이 경우는 대기하지 않고 공간이 없는 경우 reject을 반환합니다.

### Blocking Vs Non-Blocking

두 알고리즘의 차이는 요청된 동작이 수행을 할 수 없을 때 어떻게 대응하는 지에 달려있습니다. Blocking의 경우는 요청한 동작이 수행 가능해질 때까지 대기하고, Non-Blocking의 경우는 요청된 동작이 현재 수행 불가능하다고 Thread에게 Callback을 하게 됩니다.

## 출처

[양파개발자 - SW 블로그](http://oniondev.egloos.com/558949)

[박철우의 블로그](https://parkcheolu.tistory.com/33)

