---
layout: post
title: Array Vs Linked List
tags: [dataStructure]
categories: [interview]
---

## 배열(Array)

배열(array) 이란 연관된 데이터를 모아서 통으로 관리하기 위해서 사용되는 data type 입니다. 변수가 하나의 데이터를 저장하기 위해서 사용하는 것이라면 배열을 여러 개의 데이터를 하나의 변수에 저장하기 위해서 사용하는 것이라고 할 수 있습니다.

배열은 데이터를 물리적 주소에 순차적으로 저장하며, Index를 가지고 있어 바로 접근할 수 있기 때문에 접근 속도가 매우 빠릅니다. **하지만** 배열은 크기가 고정되어 있기 때문에 처음 지정된 사이즈보다 더 많은 데이터를 넣으려면 배열의 크기를 늘리는 작업이 필요합니다. 또한 데이터가 삭제될 때, 삭제된 공간을 빈 공간으로 남겨두어야 합니다. 이는 메모리 낭비를 야기합니다.

## 리스트 (Linked List)

리스트(List)는 배열(Array)가 지닌 장점 중 Index를 버리고 대신 빈틈없는 데이터를 저장하는 장점을 지닙니다. 리스트의 핵심적인 개념은 순서가 있는 데이터들의 모임이라는 점입니다. 또한 빈 데이터를 허용하지 않습니다.

연결 리스트(Linked List)는 데이터를 저장할 때 데이터만 저장하는 것이 아니라 다음 데이터의 물리적 주소까지 같이 저장합니다. 특정 데이터에 접근할 때 Index로 바로 접근할 수 있었던 배열과 달리 첫 Node 부터 원하는 노드까지 Link를 따라가야 찾을 수 있기 때문에 배열에 비해 검색 속도는 떨어집니다. **하지만** 반대로 데이터를 삽입(Insert) / 삭제(Delete) 할때는 물리적 주소에 구애받지 않고 앞 / 뒤 Node의 주소만 끼워넣을 노드의 주소로 바꿔주면 되기 때문에 삽입 / 삭제는 배열보다 빠릅니다.

![Array Vs Linked List](https://nephelai.github.io/images/posts/array_linkedlist.jpg)

## 출처

[생활코딩](https://opentutorials.org/course/743/4736)

[ahribori - 배열과 연결리스트](https://ahribori.com/article/591a5824c686bd0d48e95f47)

