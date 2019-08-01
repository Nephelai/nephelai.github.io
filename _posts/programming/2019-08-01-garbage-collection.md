---
layout: post
title: Garbage Collection
tags: []
categories: [programming]

---

## Garbage Collection 이란?

Garbage Collection(GC)란, 메모리 관리를 자동으로 처리해주는 것으로 개발자가 메모리 관리에 조금 덜 걱정하도록 도와줍니다. 메모리를 할당하려고 하면 메모리는 stack이나 heap에 할당되어집니다. 

memory block size를 알고 있는 local scope에 정의된 것들은 stack에 할당이 됩니다. Stack의 경우 스스로 메모리 관리가 이루어지므로 신경쓰지 않아도 됩니다. 하지만 heap에 정의되는 것은 관리가 필요합니다.

Heap에 위치한 메모리의 경우 프로그램의 진행동안 처리하거나 상태를 바꿀 수 있습니다. Garbage Collector는 보통 heap을 관리하는 일을 덜어줍니다.  Modern Langage 같은 경우 (JAVA, .Net framework, Python ...) garbage collected languages 이지만, C / C++의 경우는 아니므로 메모리가 관리가 개발자에게 중요한 개념입니다.

## GC의 필요성

GC는 개발자의 메모리의 중요한 Issue를 도와줍니다. 대부분의 Issue는 memory leaks입니다. heap에 많은 메모리를 할당하고 쓰지도 않은체 메모리에 그대로 둔다면, 메모리 크기는 누적될 것 입니다. 이는 heap overflow를 야기합니다. 개발자에 의해 heap memory를 관리되지만, 하나의 변수라도 삭제되지 않는다면 memory leak이 발생합니다.

memory leak이 없더라도 만약 삭제되거나 다시할당되에서 메모리 위치 참조를 시도한다면 어떨까? 이는 dangling pointer라고 합니다. 가장 좋은 Scenario는 애매한 변수가 사용될 때, 유효성 Error를 발생시키는 것입니다. GC가 완벽하게 메모리를 관리해주지는 않습니다. 하지만 적어도 메모리 관리에 신경을 덜 쓰이게 합니다

## GC가 언제, 어떻게 동작할까?

이는 GC가 쓰이는 Algorithm에 따라 다릅니다. interpreter나 compiler처럼 완벽하고 빠르게 처리는 방법은 없습니다. GC mechanisms은 점점 발전하고 있습니다. 어떤 GC는 미리 정해진 시간만큼 주기적으로 작동하기도 하며, 어떤 GC는 특정 상황에서 실행되기도 합니다. GC는 program의 한 thread에서 항상 동작합니다. GC는 program 중간에 개입을 할 수도 있습니다. 

Java에서는 JVM이 메모리를 부여 받고 프로그램 실행중에 메모리가 부족해지는 순간이 오면 OS에 추가 메모리를 요청하게 됩니다. 이 순간에 JAVA의 GC가 실행됩니다. 또한 서버 program의 경우는 지속적으로 돌아갑니다. 이 경우에는 JVM이 한가할 경우(idle time)에 GC가 실행됩니다.

## Garbage Collection Algorthims

### Reference Counting

reference counting은 아마 대부분의 GC의 기본 형태이고 가장 쉬운 구성입니다. 이 방법은 heap에 메모리 위치를 참조할 때마다 counter가 1개 증가합니다.  메모리 위치 참조가 삭제될 때마다 1개 감소합니다. 그리고 이 counter가 0이 되면 GC가 수행됩니다.

Reference Counting의 장법은 GC 동작을 바로 수행할 수 있다는 점입니다. 실시간 작업에도 거의 영향을 주지 않고 즉시 메모리에서 해제가 됩니다. 하지만 몇가지 문제를 지니고 있습니다.

1. 순환 참조 (circular reference)의 경우 GC가 실행되지 않습니다.
   * 서로 참조하여 counter가 0이 되지 않습니다.
2. Reference를 많이 하고 있는 Object의 Reference Count가 0이 될 경우 연쇄적으로 GC가 발생하는 문제가 생길 수 있습니다.
3. Reference Count 자체 관리하는 비용도 큽니다.

### Mark-Sweep

### Mark-Compact

### Copying

### Generational





## 출처

[TheCodeBoss - Programming Concepts: Garbage Collection](https://thecodeboss.dev/2017/01/programming-concepts-garbage-collection/)

[advenoh - 자바 Garbage Collection 이란](https://advenoh.tistory.com/14)

[Medium - [JVM] Garbage Collection Algorithms](https://medium.com/@joongwon/jvm-garbage-collection-algorithms-3869b7b0aa6f)