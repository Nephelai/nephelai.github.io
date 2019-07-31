---
layout: post
title: State Pattern
tags: []
categories: [designpattern]
---

## 정의

객체가 특정 상태에 따라 행위를 달리하는 상황에서, 자신이 직접 상태를 체크하여 상태에 따라 행위를 호출하지 않고, 상태를 객체화하여 상태가 행동을 할 수 있도록 위임하는 패턴입니다.

즉, 일련의 규칙에 따라 객체의 상태(State)를 변화시켜 객체가 할 수 있는 행위를 바꾸는 패턴입니다. 그래서 객체의 Class가 바뀌는 것과 같은 결과를 얻을 수 있습니다.

Class를 통해서 접근해봅시다. 객체의 특정 상태를 Class로 선언하고, Class에서는 해당 상태에서 할 수 있는 행위들을 Method로 정의합니다. 그리고 이런 각 상태 Class들을 Interface로 Encapsulation 하여 Client에서 Interface를 호출하는 방식을 말합니다.

**여기서 상태는 객체가 가질 수 있는 어떤 조건이나 상황을 의미합니다**

## 사용 이유

예를 들어 노트북을 끄고 키는 상황을 생각해봅시다.

* 노트북 전원이 켜져있는 상태에서 전원 버튼을 누르면 전원을 끌 수 있습니다.
* 노트북 전원이 꺼져있는 상태에서 전원 버튼을 누르면 전원을 킬 수 있습니다.

이런 동작을 하는 Laptop Class는 다음과 같이 정의할 수 있습니다.

```java
public class Laptop {
    public static String ON = "on";
    public static String OFF = "off";
    private String powerState = "";
    
    public Laptop() {
        setPowerState(Laptop.OFF);
    }
   	public void setPowerState(String powerState) {
        this.powerState = powerState;
    }
    public void powerPush() {
        if("on".equals(this.powerState)) System.out.println("Power on");
        else System.out.println("Power off");
    }
}
```

```java
public class Client {
    public laptop void main(String args[]) {
        Laptop laptop = new Laptop();
        laptop.powerPush();
        laptop.setPowerState(Laptop.ON);
        laptop.powerPush();
    }
}
```

간단하게 상태를 변경하는 Code를 구성했습니다. 여기서 절전모드가 추가된다고 가정해 봅시다. 절전모드에 따른 동작은 다음과 같습니다.

* 노트북 전원이 켜져있는 상태에서 전원 버튼을 누르면 전원을 끌 수 있습니다.
* 노트북 전원이 꺼져있는 상태에서 전원 버튼을 누르면 절전모드가 됩니다.
* 노트북 절전모드 상태에서 전원 버튼을 누르면 전원을 켤 수 있습니다.

절전모드가 추가된 Class는 다음과 같이 조건문이 하나 더 추가됩니다.

```java
public class Laptop {
    public static String ON = "on";
    public static String OFF = "off";
    public static String SAVING = "saving";
    private String powerState = "";
    
    public Laptop() {
        setPowerState(Laptop.OFF);
    }
   	public void setPowerState(String powerState) {
        this.powerState = powerState;
    }
    public void powerPush() {
        if("on".equals(this.powerState)) System.out.println("Power on");
        else if("saving".equals(this.powerState)) System.out.println("Power saving");
        else System.out.println("Power off");
    }
}
```

```java
public class Client {
	public laptop void main(String args[]) {
        Laptop laptop = new Laptop();
        laptop.powerPush();
        laptop.setPowerState(Laptop.ON);
        laptop.powerPush();
        laptop.setPowerState(Laptop.SAVING);
        laptop.powerPush();
        laptop.setPowerState(Laptop.OFF);
        laptop.powerPush();
	}
}
```

조건문이 하나 추가된다고 크게 불편한게 없습니다. 하지만 상태가 여러개라면 분기하는 code는 상당히 길어질 것이기 때문에 상태에 따라 하고자 하는 행위를 파악하기가 쉽지 않습니다.

또한 Laptop의 powerPush() 뿐만 아니라 TV나 SmartPhone 에서 분기가 사용된다면 기능이 변경될 때마다 일일이 찾아서 수정해야 합니다.

이렇게 상태에 따라 행위를 달리해야 하는 경우에 사용하는 패턴이 State Pattern입니다.

## State Pattern 적용

State Pattern을 적용하면 각 상태(State)들 즉, ON, OFF, SAVING 상태를 Class로 정의하고 이들을 하나의 Interface로 묶습니다. 그리고 Laptop이 State Interface의 Method를 호출하면, 각 State Class에서 정의된 행위가 수행되는 방식입니다.

![State Pattern](https://nephelai.github.io/images/posts/state_pattern.jpg)

### 전원 State를 Encapsulation한 Interface를 선언

```java
public interface PowerState {
    public void powerPush();
}
```

### PowerState Interface를 구현한 각 상태 Class 정의

``` java
public class On implements PowerState {
    public void powerPush() {
        System.out.println("Power on");
    }
}
```

``` java
public class Off implements PowerState {
    public void powerPush() {
        System.out.println("Power off");
    }
}
```

```java
public class Saving implements PowerState {
    public void powerPush() {
        System.out.println("Power saving");
    }
}
```

### Laptop Class 수정

```java
public class Laptop {
    private PowerState powerState;
    public Laptop() {
        this.powerState = new Off();
    }
    public void setPowerState(PowerState powerState){
        this.powerState = powerState;
    }
    public void powerPush(){
        powerState.powerPush();
    }
}
```

### Laptop 객체를 사용할 Client Class 정의

```java
public class Client {
    public static void main(String args[]) {
        Laptop laptop = new Laptop();
        On on = new On();
        Off off = new Off();
        Saving saving = new Saving();
        
        laptop.powerPush();
        laptop.setPowerState(on);
        laptop.powerPush();
        laptop.setPowerState(saving);
        laptop.powerPush();
        laptop.setPowerState(off);
        laptop.powerPush();
    }
}
```

결과는 똑같습니다.

## State Pattern과 Strategy Pattern 차이

결과만 바라보면 Strategy Pattern과 State Pattern이 유사한 것을 볼 수 있습니다. 거의 동일한 구조입니다.

굳이 구분을 하자면, **Strategy Pattern은 상속을 대체하려는 목적으로, State Pattern은 Code 내에 조건문들을 대체하려는 목적**으로 사용됩니다.

객체의 상태 속성을 반환하는데, State Pattern은 스스로 반환할 수 있다. 하지만 Strategy Pattern은 외부에서 Data의 입력이 필요합니다. 조금 더 구체적으로 살펴봅시다.

Strategy Pattern의 경우, Client 객체가 Context 객체에게 다른 특정 객체를 지정해주고 실행하게 합니다. Program 실행 시 Context 객체가 실행할 객체를 외부(Client 객체)에서 유연하게 지정할 수 있는 장점이 있습니다. 즉, Class의 다형성(Polymorphism)과 가상 Method(Abstract Method) 호출을 이용하여 Context Class의 Code를 간략하게 할 수 있는 방법입니다.

State Pattern의 경우, Abstract Method를 호출하여 사용한다는 점에서 유사합니다. 하지만 State Pattern의 외부(Client 객체)의 개입이 없이 상태객체 내부에서 현재 상태에 따라 Context 객체의 맴버인 State 객체를 변경하여 사용한다는 점에서 차이가 있습니다. 그러므로 State Pattern는 Program의 각 상태를 객체화하여 사용합니다.

결과적으로

* Strategy Pattern : 행동(Method, Algorithm)을 소유한 객체를 실행 시에 선택할 수 있게 하여 상황에 맞는 행동을 하는데 유용. 다양한 알고리즘을 관리하는데 유용
* State Pattern : 다양한 상태변화가 아주 복잡한 Logic을 제어하는 데 유용. if - else 문에 의한 분기점이 많아 복잡한 경우 유용

## 출처

[디자인 패턴 - 스테이트 패턴](https://victorydntmd.tistory.com/294?category=719467)

[Do it now - 디자인 패턴 state 패턴과 strategy 패턴에 대해...](https://jongyoungcha.tistory.com/entry/디자인-패턴-state-패턴과-strategy-패턴에-대해)