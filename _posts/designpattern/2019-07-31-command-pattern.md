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

```java
public class Heater {
    public void powerOn() {
        System.out.println("Heater On");
    }
}
```

```java
public class Lamp {
    public void turnOn() {
        System.out.println("Lamp On");
    }
}
```

```java
public class OKGoogle {
    private static String[] modes = {"heater", "lamp"};
    
    private Heater heater;
    private Lamp lamp;
    private String mode;
    
    OKGoogle(Heater heater, Lamp lamp) {
        this.heater = heater;
        this.lamp = lamp;
    }
    public void setMode(int idx) {
        this.mode = modes[idx];
    }
    public void talk() {
        switch(this.mode) {
            case "heater":
                this.heater.powerOn();
                break;
            case "lamp":
                this.lamp.turnOn();
                break;
        }
    }
}
```

```java
public class Client {
    public static void main(String args[]) {
        Heater heater = new Heater();
        Lamp lamp = new Lamp();
        OKGoogle okGoogle = new OKGoogle(heater, lamp);
        
        // Heater 켜짐
        okGoogle.setMode(0);
        okGoogle.talk();
        
        // Lamp 켜짐
        okGoogle.setMode(1);
        okGoogle.talk();
    }
}
```

OkGoogle에게 mode 설정을 통해 Mode 0이면 Heater를 키고, Mode 1이면 Lamp를 키도록 설정했습니다.

OKGoogle은 기능이 많아질수록 객체 property는 더욱 늘어날 것이고, 기존의 talk() Method에서 분기가 늘어날 것입니다. 이 경우 OCP도 위배 됩니다.

## Command Pattern 적용

### Encapsulation 작업 수행

먼저 OKGoogle이 할 수 있는 기능을 Class로 만들어서 각 기능들을 Encapsulation

* HeaterOnCommand / LampOnCommand

OKGoogle Class의 talk() Method에서 heater.powerOn(), lamp.turnOn() 같이 기능을 직접 호출하지 않고 Capsulation 된 Command Interface의 Method를 호출하도록 합니다.

![Command Pattern](https://nephelai.github.io/images/posts/command_pattern.jpg)

### Interface 정의

``` java
public interfave Command() {
    public void run();
}
```

### HeaterOnCommand Class와 .powerOn() Method 정의

Heater를 켜는 명령을 Class화(Encapsulation)하고 Heater 키는 명령을 정의합니다.

```java
public class HeaterOnCommand implements Command {
    private Heater heater;
    public HeaterOnCommand(Heater heater) {
        this.heater = heater;
    }
    public void run() {
        heater.powerOn();
    }
}
```

```java
public class Heater {
    public void powerOn() {
        System.out.println("Heater On");
    }
}
```

### LampOnCommand Class와 .turnOn() Method 정의

마찬가지로 Lamp를 키는 명령을 Class화(Encapsulation)하고 Heater 키는 명령을 정의합니다.

```java
public class LampOnCommand implements Command {
    private Lamp lamp;
    public LampOnCommand(Lamp lamp) {
        this.lamp = lamp;
    }
    public void run() {
        lamp.turnOn();
    }
}
```

### OKGoogle Class의 talk() Method에서는 Command Interface의 run() Method 실행

```java
public class OKGoogle {
    private Command command;
    public void setCommand(Command command) {
        this.command = command;
    }
    public void talk() {
        command.run();
    }
}
```

### 마지막으로 OKGoogle을 사용하는 Client Class를 정의합니다.

```java
public class Client {
    public static void main(String args[]) {
        Heater heater = new Heater();
        Lamp lamp = new Lamp();
        
        Command heaterOnCommand = new HeaterOnCommand(heater);
        Command lampOnCommand = new LampOnCommand(lamp);
        OKGoogle okGoogle = new OKGoogle();
        
        // Heater 켜짐
        okGoogle.setCommand(heaterOnCommand);
        okGoogle.talk();
        // Lamp 켜짐
        okGoogle.setCommand(lampOnCommand);
        okGoogle.talk();
    }
}
```

만약 여기에 더욱 추가되는 내용이 있어도 쉽게 유지보수가 이루어질 수 있습니다.

## 출처

[디자인패턴 - 커맨드 패턴](https://victorydntmd.tistory.com/295)