---
layout: post
title: Difference Class, Interface, Abstract Class
tags: []
categories: [programming]
---

JAVA의 경우 객체 지향언어로 Class를 기반으로 동작합니다. 모든 것들이 Class 위주로 돌아갑니다.

## Class

Class란 **유사한 특징을 지닌 객체들의 속성을 묶어 놓은 집합체**입니다.

### Object

Software에서 구현할 대상 / Class에 선언된 모양 그대로 생성된 실체

**Class의 Instance**라고도 부릅니다. 객체는 모든 Instance를 대표하는 포괄적인 의미를 갖습니다. 또한 oop 관점에서 Class의 Type으로 선언되었을 때, 객체라고 부릅니다.

### Instance

설계도를 바탕으로 Software에 구현된 구체적인 실체 즉, 객체를 Software에 실체화를 하면 그것을 Instance라고 부릅니다. 실체화가 이루어진 **Instance는 Memory에 할당**됩니다.

Instance는 객체에 포함되어 있습니다. oop의 관점에서 객체가 memory에 할당되어 실제 사용될 때 Instance 라고 부릅니다.

추상적인 개념(또는 Specification)과 구체적인 객체 사이의 관계에 초점을 맞출 경우 사용합니다.

* 객체는 Class의 Instance 입니다.
* 객체 간의 Link는 Class 간의 연관 관계의 Instance 입니다.
* 실행 process 는 program의 Instance 입니다.

```java
public class Vehicle { ... } // class
public class Main {
    public static void main(String[] args) {
        Vehicle bus, texi; // Object, 아직 실체화는 되어 있지 않다.
        
        bus = new Vehicle();
        texi = new Vehicle(); // Instance, 사용할 수 있는 준비가 되었다. 실제적으로 Memory에 할당이 되어진다.
    }
}
```

## Abstract Class

Class내에 Abstract Method가 하나 이상 포함되어있거나 abstract로 정의된 경우를 말합니다. 선언만 있고 구현 내용이 없어서 Abstract Class만으로는 새로운 Instance를 생성할 수 없습니다.

### Abstract Class 용도

1. **공통된 Field와 Method를 통일할 목적** : 유지보수성을 높이고 통일성을 유지할 수 있습니다.
2. **Concrete Class 구현시 시간 절약** : 주어지는 Field와 Method를 가지고 나만의 Style대로 구현할 수 있습니다. 즉, 설계 시간이 절약되고, 구현하는데만 집중할 수 있습니다.
3. **규격에 맞는 Concrete Class 구현** : 아무리 마음대로 구현한다고 해도 결국엔 규격안에서 구현하는 것을 허락합니다. 규격을 벗어날 수는 없습니다.

만약 어떤 Abstract Class를 상속 받은 자식 Class에서 Abstract Method를 구현하지 않았다면 자식 Class도 Abstract Class가 되어야 합니다.

## Interface

간단하게 생각하면 구현된 것은 아무 것도 없이 밑그림만 있는 기본 설계도입니다. Interface의 가장 큰 장점은 여러 Interface로 부터 상속이 가능합니다. (다중 상속)

Interface 또한 이 자체로는 Instance를 생성할 수 없으며, 서로 관계가 없는 Class들에게 관계를 맺어줄 수 있습니다. 반대로 직접적인 관계를 간접적인 관계로 바꾸어 줄 수도 있습니다.

## Abstract Class VS Interface

Abstract Class는 그 Abstract Class를 상속 받아서 기능을 이용하고 확장하는 시키는데 존재 목적이 있습니다.

Interface는 함수의 설계도만 있습니다. 그 이유는 함수의 구현을 강제하기 위해서 입니다. 이렇게 구현을 강제함으로써 구현한 객체의 같은 동작을 보장할 수 있습니다.

또한 JAVA가 다중 상속을 지원하지 않기 때문에 Interface를 이용합니다. 하지만 여기서도 문제가 발생할 수 있습니다. 이를 살펴보기 전에 Abstract Class와 Interface의 존재 이유를 살펴볼 필요가 있습니다.

상속은 SuperClass의 기능을 이용하거나 확장하기 위해서 사용되고, 다중 상속의 모호성 때문에 하나만 상속받을 수 있습니다.

반면 Interface는 해당 Interface를 구현한 객체들에 대해서 동일한 동작을 약속하기 위해 존재합니다.

따라서 Abstract Class와 Interface의 차이 및 존재 이유는 **외형적인 차이뿐만 아니라 상속이라는 개념과 다형성이라는 개념**을 알고 있는지에 대한 질문이 되기도 합니다.



## 출처

[brunch - 자바의 추상 클래스와 인터페이스](https://brunch.co.kr/@kd4/6)

[HAMA 블로그 - 인터페이스 vs 추상 클래스](https://hamait.tistory.com/650)

[기본기를 쌓는 정마추어 코딩블로그](https://jeong-pro.tistory.com/82)

