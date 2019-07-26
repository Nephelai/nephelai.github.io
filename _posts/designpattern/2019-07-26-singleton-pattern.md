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

이 경우 외부에서 Single



## 출처

[Tistory Blog - 싱글톤 패턴](https://victorydntmd.tistory.com/293?category=719467)