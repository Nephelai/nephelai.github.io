---
layout: post
title: Factory Method Pattern
tags: []
categories: [designpattern]
---

## 정의

어떤 상황에서 조건에 따라 객체를 다르게 생성해야 할 때가 있습니다. 예를 들면 사용자의 입력값에 따라 하는 일이 달라지는 경우 조건 등을 이용해 특정 객체를 생성해야 합니다.

Factory Method Pattern은 조건에 따른 객체 생성(new 연산자로 객체를 생성하는 부분)을 직접하는 것이 아니라 Factory라는 Class에 위임하여 객체를 생성하도록 하는 방식을 말합니다.

즉, Factory는 객체를 찍어내는 공장을 의미합니다.

## 사용 이유

 위의 예시를 그대로 이용하여 코드를 작성해 보았습니다.

```java
public abstract class Type {}
```

```java
public class TypeA extends Type {
    public class TypeA() {
        System.out.println("Create Type A");
    }
}
```

```java
public class TypeB extends Type {
    public class TypeB() {
        System.out.println("Create Type B");
    }
}
```

```java
public class TypeC extends Type {
    public class TypeC() {
        System.out.println("Create Type C");
    }
}
```

```java
public class ClassA {
    public Type createType(String type) {
        Type returnType = null;
        switch(type) {
            case "A":
                returnType = new TypeA();
                break;
            case "B":
                returnType = new TypeB();
                break;
            case "C":
                returnType = new TypeC();
                break;
        }
        return returnType;
    }
}
```

```java
public class Client {
    public static void main(String args[]) {
        ClassA classA = new ClassA();
        classA.createType("A");
        classA.createType("C");
    }
}
```

TypeA, TypeB, TypeC Class를 정의하고 Type이라는 Abstract Class 를 정의하여 캡슐화를 했습니다. 그 후 ClassA의 createType() Method를 통해 문자열에 따라 Type Class 생성합니다.

하지만 여러 Class에서 switch 구문을 사용하는 경우라면 어떻게 될까요? ClassA만 존재하는 것이 아니라, ClassB, ClassC가 존재한다면 이 또한 중복된 코드가 발생할 수 있습니다.

또한 객체를 생성하는 일은 객체간의 결합도를 강하게 만드는 일이 되고 객체간의 결합도가 강하면 유지보수는 어려워집니다.

Factory Method Pattern을 이용하여 객체를 생성하는 부분은 Factory Class를 만들어서 Factory Class에게 객체 생성을 위임할 것입니다.

## Factory Method Pattern 적용

2가지의 방법을 통해서 적용할 수 있습니다.

* Factory Class 정의
* 객체 생성이 필요한 Class에서 Factory 객체를 생성하여 조건에 따른 객체 생성 Method 호출

Type, TypeA, TypeB, TypeC, Client 클래스는 동일하고 Factory 클래스와 이를 이용하는 ClassA 클래스를 구현해보겠습니다.

```java
public class TypeFactory {
    public Type createType(String type) {
        Type returnType = null;
        switch(type) {
            case "A":
                returnType = new TypeA();
                break;
            case "B":
                returnType = new TypeB();
                break;
            case "C":
                returnType = new TypeC();
                break;
        }
        return returnType;
    }
}
```

```java
public class ClassA {
    public Type createType(String type) {
        TypeFactory factory = new TypeFactory();
        Type returnType = factory.createType(type);
        return returnType;
    }
}
```

이렇게 조건에 따른 객체 생성 부분을 자신이 직접하지 않고 Factory Class에 위힘하여 객체를 생성하도록 하는 방법을 Factory Method Pattern이라 합니다. 이를 통해 객체간의 결합도가 낮아지고 유지보수에 용이해집니다.

## 출처

[victolee - 팩토리 메서드 패턴(Factory Method Pattern)](https://victorydntmd.tistory.com/299?category=719467)