---
layout: post
title: Template Method Pattern
tags: []
categories: [designpattern]

---

## 정의

템플릿 메서드 패턴은 여러 클래스에서 공통으로 사용하는 Method를 상위 Class에서 정의하고, 하위 Class 마다 다르게 구현해야하는 세부적인 사항을 하위 Class에서 구현하는 패턴을 의미합니다.

즉, 코드의 중복을 제거하기 위해서 사용하는 Refactoring 기법입니다.

## 사용 예시

부모 Class에서 공통적으로 사용하는 부분을 someMethod라고 하며 이를 Template Method라 합니다.

```java
public class Parent {    
    public void someMethod() {
        System.out.println("This is Parent");// 공통적인 작업
        hook(); // Child에서 구현해야 할 부분
    }
    
    public void hook() {};
}
```

```java
public class ChildA extends Parent {
    @Override
    public void hook() {
        System.out.println("Implement hook in ChildA");
    }
}
```

```java
public class ChildB extends Parent {
    @Override
    public void hook() {
        System.out.println("Implement hook in ChildB");
    }
}
```

```java
public class Client {
    public static void main(String args[]) {
        ChildA childA = new ChildA();
        childA.someMethod();
        
        ChildB childB = new ChildB();
        childB.someMethod();
    }
}
```



## 출처

[victolee - 템플릿 메서드 패턴(Template Method Pattern)](https://victorydntmd.tistory.com/298?category=719467)