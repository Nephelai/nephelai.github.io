---
layout: post
title: Command Pattern
tags: []
categories: [designpattern]
---

## 정의

객체의 행위(Method)를 Class로 만들어 Encapsulation 하는 패턴입니다.

만약 어떤 객체(A)에서 다른 객체(B)의 Method를 실행하려면 그 객체(B)를 참조하고 있어야 하는 의존성이 발생합니다. 그러나 Command Pattern을 이용하면 이 의존성을 제거할 수 있습니다.

또한 기능이 수정되거나 변경이 일어날 경우 A Class 코드를 수정없이 기능에 대한 Class를 정의하면 되므로 System 확장성이 높고 유연해집니다.

## 사용 이유

IF> 'Google Home' 이라는 구글 서비스가 있습니다. 이는 "OK Google 히터 틀어줘"하면, 히터를 틀어 줍니다. 이를 이용해 Class를 정의해 봅시다.

* Google Home을 사용하는 사용자를 Client Class
* Google Home을 OKGoogle Class
* 히터를 Heater Class

### OKGoogle은 Heater을 켜기 위해 Heater 객체를 참조합니다.

```java
public class Heater {
    public void powerOn() {
        System.out.println("Heater On");
    }
}
```

```java
public class OKGoogle {
    private Heater heater;
    public OkGoogle(Heater heater) {
        this.heater = heater;
    }
    public void talk() {
        heater.powerOn();
    }
}
```

```java
public class Client {
    public static void main(String args[]) {
        Heater heater = new Heater();
        OKGoogle okGoogle = new OKGoogle(heater);
        okGoogle.talk();
    }
}
```

하지만 Heater 켜는 기능 말고, Lamp를 켜는 기능을 추가하고 싶다면 어떻게 수정할까요?

위의 Lamp Class를 정의하고 OKGoogle Class에서 Lamp 객체를 참조하여야 합니다. 































## 출처

[디자인패턴 - 커맨드 패턴](https://victorydntmd.tistory.com/295)