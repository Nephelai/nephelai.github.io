---
layout: post
title: HashTable
tags: [dataStructure]
categories: [interview]

---

## Concept

**해싱(hashing)**이란 mapping 전 원래 데이터의 값(**key**)를 mapping 후 데이터의 값(**hash value**)로 mapping하는 과정 자체를 의미합니다. 이 mapping 하는 과정, 즉 이때에 쓰는 함수를 **해시함수(hash function)**라 합니다. 조금 더 살펴보면 **해시함수(hash function)**는 데이터의 효율적인 관리를 목적으로 임의의 길이의 데이터를 고정된 길이의 데이터로 mapping하는 함수입니다.

**hash function**은 대체적으로 많은 key를 hash value로 변환(many to one mapping)하기 때문에 hash function이 서로 다른 두 key에 대해 동일한 hash value를 가르키는 **hash collision**이 발생합니다.

## Hash Table

Hash function을 사용하여 key를 hash value로 mapping하고, 이 hash value를 index로 하여 데이터의 원래 값(value)와 함께 저장하는 자료구조를 **hash table**이라고 합니다. 이 때, 데이터가 저장되는 곳을 **bucket 또는 slot**이라고 합니다. 

![Hash Table](https://nephelai.github.io/images/posts/hashtable.jpg)

## Hash Table의 장점

### 적은 Resource로 많은 데이터를 효율적으로 관리할 수 있습니다.

예를 들어 해시 함수로 HardDisk에 존재하는 많은 데이터(Key)를  제한된 개수의 Hash Value에 mapping하므로써 작은 크기의 memory로 process를 관리할 수 있습니다.

### 데이터에 대한 INSERT / DELETE / SEARCH가 빠르게 수행됩니다.

hash function은 언제나 동일한 hash value를 리턴하고 해당 index만 알면 hash table의 크기에 상관없이 데이터에 빠르게 접근할 수 있습니다. 즉, data access의 시간이 빠릅니다.

## Hash Collision의 해결 방안

### Open  Address 방식 (개방 주소법)

Hash collision이 발생하면 다른 hash bucket에 해당 자료를 삽입하는 방식입니다. 즉, 이 알고리즘은 Collision이 발생하면 데이터를 저장할 장소를 찾아 헤맵니다. Worst Case의 경우 Bucket을 찾지 못하고 탐색을 시작한 위치까지 되돌아 올 수 있습니다. 이 과정에도 크게 3가지가 존재합니다.

1. Linear Probing : 순차적으로 탐색과정이 이루어 집니다.
2. Quadratic Probing : 2차원 조사로 $H(x) = (a*x + b + i^2)$ $mod$ $p$ 의 형태입니다. 탐색 과정이 $i^2$으로 넘어가면서 이루어집니다.
3. Double Hashing : 이중해싱으로 $H(x) = (a*x + b + i*f(x))$ $mod$ $p$  의 형태입니다. 이때의 $f(x)$ 또한 $c*x + d$ 같은 형태로 나타낼 수 있습니다.  탐색 과정이 $i*f(x)$으로 넘어가면서 이루어집니다.

### Separate Chaining 방식 (분리 연결법)

일반적으로 Open Addressing 은 Separate Chaining 보다 느립니다. Open Addressing의 경우 Hash Bucket의 채운 값이 많아 질수록 Worst Case 발생 빈도가 더 높아지기 때문입니다. 하지만 Separate Chaining 방식의 경우 Hash Collision이 잘 발생하지 않도록 보조 Hash Function을 통해 조절할 수 있다면 Worst Case에 가까워지는 빈도를 줄일 수 있습니다. *Java 7의 HashMap은 Separate Chaining 방식을 사용하여 구현되어 있습니다.* 이 과정으로는 2가지의 구현 방식이 존재합니다.

#### Using Linked List

각각의 Bucket들을 Linked List로 만들어 Collision이 발생하면 해당 bucket의 list에 추가하는 방식입니다. Linked List의 특징을 그대로 이어받아 삽입과 삭제가 간단한 반면, 작은 데이터를 저장할 때, Linked List 자체의 데이터(meta data)가 overhead로 부담될 수 있습니다.

#### Using Tree(Red-Black Tree)

기본적인 알고리즘은 Separate Chaining방식과 동일하며 Linked List 대신 Tree를 이용합니다. Tree를 기본적으로 Memory 사용량이 많기 때문에 데이터의 개수가 적다면 Linked List 사용하는 것이 옳습니다. 하지만 데이터가 많아지면 Tree가 더 효율적입니다.

### Open Address VS Separate Chaining

두 방식 모두 Worst Case에서 시간복잡도는 선형시간으로 동일합니다. 하지만 Open Address 방식은 연속된 공간에 데이터를 저장하기 때문에 Separate Chaining에 비해 Cache 효율이 높습니다. 하지만 Open Address 은 Bucket을 계속해서 사용하지 않기 때문에 Separate Chaining 방식에 비해 테이블의 확장이 빨리 발생합니다.

## Hash Bucket의 동적 확장(Resize)

Hash Bucket의 개수가 적을 경우 memory의 사용을 아낄 수 있지만 Hash Collision으로 인해 성능에서의 손실이 발생합니다. 그래서 HashMap의 경우 key-value의 데이터 개수가 일정 개수 이상이 되면 Bucket의 개수를 2배로 늘립니다. 이렇게 Bukcet의 크기를 2배로 늘리는 과정을 Resizing이라고 합니다.

## 출처

[조대협의 블로그 - 해쉬 테이블의 이해와 구현](https://bcho.tistory.com/1072)

[어제보다 한 걸음 더 - Hash Table 이란?](https://k39335.tistory.com/18)