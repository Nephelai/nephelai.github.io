---
layout: post
title: Heap
tags: [dataStructure]
categories: [interview]

---

## Heap

Heap은 Max 및 Min Value를 찾아내는 연산을 빠르게 수행하기 위해 만들어졌습니다. Complete Binary Tree를 바탕으로 다음과 같은 속성을 만족합니다.

* 특정 Node에 대해 Parent Node와의 대소 관계가 존재(smaller or larger)

Parent - Child 간의 대소 관계만 존재할 뿐 Sibling 간에는 대소 관계가 정해지지 않습니다. 또한 중복된 값을 허용합니다.

### Heap의 종류

크게 2가지로 나뉠 수 있습니다.

* Max Heap : Parent Node의 Key value가 Child Node의 Key value보다 크거나 같은 Complete Binary Tree / `Parent's Node >= Child's Node`
* Min Heap : Parent Node의 Key value가 Child Node의 Key value보다 작거나 같은 Complete Binary Tree / `Parent's Node <= Child's Node`

### Heap의 구현

Heap을 저장하는 표준적인 자료구조는 Array입니다. 구현을 보다 쉽게 하기 위해서 배열의 첫 Index인 0은 사용하지 않습니다. 

Heap에서 Parent Node와 Child Node의 관계

* Left Child Index = Parent Index $*~2$
* Right Child Index = Parent Index $*~2+1$
* Parent Index = Child Index $ / ~ 2$

![Heap Structure](https://nephelai.github.io/images/posts/heap_structure.jpg)

## Heap과 Priority Queue

**Priority Queue**는 Abstract Data Type(ADT) 중 하나입니다. 따라서 '어떤 일을 할 수 있는가'에 초점을 맞추어 접근합니다.

* Queue에 element를 priority를 연관시켜서 넣을 수 있습니다.
* Queue에서 가장 높은 priority의 element를 꺼낼 수 있습니다.

**Heap**은 자료구조로써 Priority Queue의 구현체입니다. 정확하게는 Binary Heap의 Array Implementation입니다.

Abstract Data Type이 문제에 초점을 두고 있다면, Data Structure은 해답에 초점을 두고 있습니다. Interface와 Implementation의 관계와 비슷하게 생각할 수 있습니다.

## 출처 

[[자료구조] 힙(heap)이란](https://gmlwjd9405.github.io/2018/05/10/data-structure-heap.html)

[위키디피아 - Heap](https://ko.wikipedia.org/wiki/힙_(자료_구조))

[킹포도의 코딩 - 자료구조 힙(Heap)](https://kingpodo.tistory.com/30)

[maronote - 우선순위 큐와 힙](http://maronote.blogspot.com/2013/07/priority-queue-heap.html)