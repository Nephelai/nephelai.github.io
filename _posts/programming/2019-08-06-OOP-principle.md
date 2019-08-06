---
layout: post
title: OOP Principle with SOLID
tags: []
categories: [programming]

---

## OOP란?

객체지향 프로그래밍(Object-Oriented Programming, OOP)은 조금 더 나은 프로그램을 만들기 위한 프로그래밍 패러다임으로 로직을 상태(State)와 행위(Behava, Method)로 이루어진 객체로 만드는 것입니다. 이런 객체들을 조립(재구성)하여 하나의 프로그램을 만드는 것이 객체지향 프로그래밍이라고 할 수 있습니다.

이런 객체지향 프로그램의 5대 원칙이 존재합니다.

1. Single Responsiblity Principle(SRP) : 단일 책임 원칙
2. Open-Closed Principle(OCP) : 개방-폐쇄 원칙
3. Liskov Substitution Principle(LSP) : 리스코프 치환 원칙
4. Dependency Inversion Principle(DIP) : 의존 역전 원칙
5. Interface Segregation Principle(ISP) : 인터페이스 분리 원칙

### Single Responsiblity Principle

**Software의 설계 부품(Class, Function 등)은 단 하나의 책임만을 지녀야 한다.**

설계가 잘 이루어진 프로그램은 기본적으로 새로운 요구사항과 프로그램 변경에 영향을 받는 부분이 적습니다. 다시 말해, 응집도는 높고 결합도는 낮은 프로그램을 의미합니다. 만약 한 클래스가 수행할 수 있는 기능 즉, 책임이 많아지는 경우 클래스 내부의 함수끼리의 강한 결함이 발생할 가능성이 높아지고 이는 결과적으로 유지보수 비용이 증가하게 됩니다. 그래서 책임을 분리시킬 필요가 있습니다.

### Open-Closed Principle

**기존의 코드를 변경하지 않고(Closed) 기능을 수정하거나 추가할 수 있도록(Open) 설계해야 한다.**

OCP에 만족하는 설계를 할 때, **변경되는 것이 무엇인가**에 초점을 맞추어야 합니다. 자주 변경되는 내용은 수정하기 쉽게 설계하고, 변경되지 않아야 하는 것은 수정되는 내용에 영향을 받지 않게 하는 것이 포인트입니다. 이를 위해 자주 사용되는 문법이 **Interface**입니다.

``` java
class SoundPlayer {
    void play() {
        System.out.println("play wav");
    }
}
public class Client {
    public static void main(String[] args) {
        SoundPlayer sp = new SoundPlayer();
        sp.play();
    }
}
```

SoundPlayer는 기본적으로 wav 파일을 재생할 수 있습니다. 하지만 SoundPlayer가 만일 다른 format의 파일을 지원하게 요구사항이 변경되었다고 생각해봅시다. 요구사항을 만족하기 위해서 SoundPlayer의 play() Method를 수정하여야 합니다. 하지만 이는 OCP 원칙에 위배됩니다.

그렇다면 어떻게 접근해야 OCP 원칙을 만족할 수 있을까요? 위에서 언급한 Insterface를 이용해봅시다.

```java
interface playAlgorithm {
    public void play();
}
class Wav implements playAlgorithm {
    @Override
    public void play() {
        System.out.println("Play Wav");
    }
}
class Mp3 implements playAlgorithm {
    @Override
    public void play() {
        System.out.println("Play Mp3");
    }
}
```

```java
class SoundPlayer {
    private playAlgorithm file;
    public void setFile(playAlgorithm file) {
        this.file = file;
    }
    public void play() {
        file.play();
    }
}
public class Client {
    public static void main(String[] args) {
        SoundPlayer sp = new SoundPlayer();
        sp.setFile(new Wav());
        sp.setFile(new Mp3());
        sp.play();
    }
}
```

이와 같은 설계를 Strategy Pattern 이라고 합니다.

결과적으로 우리는 SoundPlayer Class의 변경 없이 재생되는 파일을 바꿀 수 있으므로 OCP를 만족합니다. OCP를 만족하는 설계는 변경에 유연하므로 유지보수 비용을 줄여주고 코드의 가독성 또한 높아지는 효과를 얻을 수 있습니다.

### Liskov Substitution Principle

**자식 Class는 부모 Class에서 가능한 행위(Method)를 수행할 수 있어야 한다**

이는 MIT Computer Science 교수인 Liskov가 제안한 설계 원칙으로 부모 Class와 자식 Class 사이의 행위에는 일관성이 있어야 한다는 원칙입니다. 이는 객체 지향 프로그래밍에서 부모 Class의 Instance 대신 자식 Class의 Instance를 사용해도 문제가 없어야 한다는 것을 의미합니다.

상속 관계에서는 일반화 관계(IS-A, `~은 ~이다 라는 관계 `) 성립해야 합니다. 일반화 관계에 있다는 것은 일관성이 있다는 것을 의미합니다. 따라서 Liskov 치환 원칙은 일반화 관계에 대해 언급하는 것이라고 할 수 있습니다.

### Dependency Inversion Principle

**의존 관계를 맺을 때, 변화하기 쉬운 것보단 변화하기 어려운 것에 의존해야 한다**

여기서 말하는 변화하기 쉬운 것은 구체적인 것(concrete)을 의미하며, 변화하기 어려운 것(abstract)을 의미합니다. 따라서 **DIP를 만족한다는 것은 의존 관계를 맺을 때, Concrete Class 보다 Abstract Class, Interface와 관계를 맺는다는 것을 의미합니다.

Interface를 상속받아 구현한 후 Method(setFile 등)를 이용하여 멤버 변수에 주입시킬 수 있습니다. 이와 같이 주입시키는 기술을 `의존성 주입`이라고 합니다.

### Interface Segregation Principle

**한 Class는 자신이 사용하지 않는 Interface는 구현하지 말아야 한다. 하나의 일반적인 Interface 보다는 여러 개의 구체적인 Interface가 낫다.**

즉, 자신이 사용하지 않는 기능(Interface)에는 영향을 받지 말아야 한다는 의미입니다.

프로그램을 구상할 때, 서로에게 영향을 주는 기능들은 각각의 독립된 Interface로 구현하여 서로에게 영향을 받지 않도록 설계를 하여야 합니다. 이렇게 설계된 Software는 ISP를 통해 System의 내부 의존성을 약화시켜 Refactoring, Update, reDeploy를 쉽게 할 수 있습니다.

## 출처

[객체지향 프로그래밍 - 생활코딩](https://opentutorials.org/course/743/6553)

[Programming Note - SOLID 원칙](https://dev-momo.tistory.com/entry/SOLID-원칙)