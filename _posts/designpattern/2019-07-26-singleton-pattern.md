---
layout: post
title: Singleton Pattern
tags: []
categories: [designpattern]


---

## 정의

Instance가 Program 내에서 오직 하나만 생성되는 것을 보장하고, Program 어디에서든 이 Instance에 접근할 수 있도록 하는 Pattern. 즉, Instance가 사용될 때, 똑같은 Instance를 여러 개 만드는 것이 아닌, 기존에 생성했던 동일한 Instance를 사용하게 구성합니다.

## 구현

``` java
public class SingleObj {
    // 정적 type으로 생성
    private static SingleObj singleOgj = null;
    
    // 외부에서 직접 생성하지 못하도록 한다.
    private SingleObj(){}
    
    // 오직 1개의 Object(Instance)만 생성
    public static SingleObj getInstance() {
        if(singleObj == null) {
            singleObj = new Single();
        }
        return singleObj;
    }
}
```

외부에서 객체를 생성할 수 없도록 생성자를 private로 선언합니다. 즉, 객체 생성에 대한 과니를 내부적으로 관리합니다.

이 경우 외부에서 SingleObj를 생성할 수 없으므로, 미리 생성된 자신을 반환할 수 있도록 getInstance()를 정의합니다. 주의해야 할 것은 static으로 정의가 되어있습니다. 생성자를 private으로 선언했기 때문에 객체를 생성할 수 없으므로, getInstance() Method가 Class에 정의되도록 static 제어자를 사용했습니다.

getInstance() Method 호출 시,

* singleObj 변수에 객체가 할당되지 않으면(== null) 새로운 객체를 생성하고,
* singleObj 변수에 객체가 이미 있으면 그것을 그대로 반환합니다.

MultiThread 환경에서 Singleton을 처리하는 방법에 대해 알아보자.

## Multi Thread 환경

Single Thread에서의 사용되는 경우 문제가 되지 않지만, Multi Thread 환경에서 Singleton 객체에 접근 시 초기화 관련하여 문제가 발생합니다.

**어떻게 Code를 작성해야 Singleton 객체를 생성하는 로직을 thread-safe하게 적용할 수 있을까?**

정말 단순하게 짜고 싶다면 `synchronized` keyword를 추가해도 됩니다. 하지만 이 방법은 역할에 비해서 동기화 오버헤드가 심하게 발생합니다.

그래서 개발자들 사이에서 Singleton 초기화에 관련된 많은 idiom이 존재합니다.

### Double Checked Locking

일명 DCL이라고 불리는 이 기법은 현재 broken idiom이며, 사용을 권고하지는 않습니다.

``` java
public class singleton {
    private volatile static Singleton instance;
    private Singleton() {}
    
    public static Singleton getInstacne() {
        if(instance == null) {
            synchronized(Singleton.class) {
                if(instance == null) instance == new Singleton();
            }
        }
        return instance;
    }
}
```

Method에 `synchronized`를 제거하여 동기화 오버헤드를 줄여보자는 의도로 설계된 idiom 입니다. `instance`가 `null`인지 확인하고, `null`인 경우 동기화 Block에 진입하게 됩니다. 그래서 최초 객체가 생성된 이후로는 동기화 블럭에 진입하지 않기 때문에 효율적인 idiom이라고  생각할 수 있습니다. 하지만 아주 않좋은 Case로 정상 동작을 하지 않을 수 있습니다. 그래서 broken idiom이라고 합니다.

`Thread A`와 `Thread B` 가 있습니다. `Thread A`가 `instance`의 생성을 완료하기 전에 메모리 공간에 할당이 가능하게 때문에 `Thread B`가 할당되어 있는 것을 보고 `instace`를 사용하려고 하나, 아직 생성은 이루어지지 않아서, 즉 생성과정이 모두 끝나지 않았기 때문에 사용하려고 하여 오작동이 발생할 수 있습니다. 이런 확률은 적겠지만 혹시 모를 문제를 지니고 있어 쓰지 않는 것이 좋습니다.

### Enum

Java에 공헌을 많이한 Joshua Bloch가 언급한 idiom으로 간단하게 **`class`가 아닌 `enum`으로 정의하는 것 입니다.**

``` java
public enum Singleton {
    INSTANCE;
}
```

`enum`은 Instance가 여러개 생기지 않도록 확실하게 보장해주고 복잡한 직렬화(serialization)나 리플렉션(reflection) 상황에서도 직렬화가 자동으로 지원된다는 이점이 있습니다.

하지만 enum에도 한계가 존재합니다. 보통 Andriod 개발을 하게 될 경우 보통 Singleton 초기화 과정에 `Context`라는 의존성이 끼어들 가능성이 높습니다. `enum`의 초기화는 Compile Time에 결정이 되므로 매번 Method 등을 호출할 때 `Context` 정보를 넘겨야하는 비효율적인 상황이 발생할 수 있습니다. 

결론은 `enum`은 효율적인 idiom이지만 상황에 따라 사용이 어려울 수도 있습니다.

### LazyHolder

현재까지 가장 완벽하다고 평가받는 idiom입니다.

```java
public class Singleton {
    private Singleton() {}
    public static Singleton getInstance() {
        return LazyHolder.INSTANCE;
    }
    
    private static class LazyHolder {
        private static final Singleton INSTANCE = new Singleton();
    }
}
```

**이 idiom은 객체가 필요할 때로 초기화를 미루는 것입니다. Lazy Initialization이라고도 합니다.**

`Singleton`Class에서는 `LazyHolder`Class의 변수가 없기 때문에 `Singleton`Class Loading시 `LazyHolder`Class를 초기화하지 않습니다. `LazyHolder`Class는 `Singleton`Class의 `getInstance()`Method에서 `LazyHolder.INSTANCE`를 참조하는 순간 Class가 로딩되며 초기화가 진행됩니다. Class를 Loading하고 초기화하는 시점은 thread-safe를 보장하기 때문에 volatile이나 synchronized 같은 keyword가 없어도 thread-safe하면서 성능도 보장하는 아주 좋은 idiom이라고 할 수 있습니다.

## 출처

[Tistory Blog - 싱글톤 패턴](https://victorydntmd.tistory.com/293?category=719467)

[Multi Thread 환경에서의 올바른 Singleton - Medium](https://medium.com/@joongwon/multi-thread-환경에서의-올바른-singleton-578d9511fd42)