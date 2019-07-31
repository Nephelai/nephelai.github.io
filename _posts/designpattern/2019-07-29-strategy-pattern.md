---
layout: post
title: Strategy Pattern
tags: []
categories: [designpattern]
---

## 정의

객체들이 할 수 있는 행위 각각에 대해 전략(Strategy) Class를 생성하고, 유사한 행위들을 Encapsulation하는 Interface를 정의하여, 객체의 행위를 동적으로 바꾸고 싶은 경우 직접 행위를 수정하지 않고 전략(Strategy)을 바꿔주기만 함으로써 행위를 유연하게 확장하는 방법을 말합니다.

간단히 말해서 객체가 할 수 있는 행위들을 각각 전략으로 만들어 놓고, 동적으로 행위의 수정이 필요한 경우 전략을 바꾸는 것만으로 행위의 수정이 가능하도록 만든 패턴입니다.

![Strategy Pattern](https://nephelai.github.io/images/posts/strategy_pattern.jpg)

```java
public interface Movable {
    public void move();
}
```

``` java
public class Train implements Movable {
    public void move() {
        System.out.println("Using Track...");
    }
}
```

```java
public class Bus implements Movable {
    public void move() {
        System.out.println("Using Road...");
    }
}
```

``` java
public class Client {
    public static void main(String args[]) {
        Movable train = new Train();
        Movable bus = new Bus();
        
        train.move();
        bus.move();
    }
}
```

하지만 나중에 Tracks을 따라 움직이는 버스가 개발되었다고 하면 Bus의 `move()`를 바꾸어 주면 됩니다.

하지만 이런 방식으로 수정하는 것은 **OOP의 SOLID 원칙 중 OCP(Open-Closed Principle)에 위배**됩니다. OCP에 의하면 기존의 `move()`를 수정하지 않으면서 행위(move)가 수정되어야 하지만 지금은 Bus의 `move()` Method를 직접 수정했습니다.

지금과 같은 방식의 변경은 System이 확장되었을 때, 유지보수를 어렵게 합니다. 예를 들어, Bus와 같이 Road를 따라 움직이는 Taxi, Car, Bike 등이 추가된다고 할 때, 모두 Bus와 같이 `move()` Method를 사용합니다.

만약에 새로 개발된 Road를 따라 움직이는 Bus와 같이, Road를 따라 움직이는 Bus, Taxi 등이 생긴다면, 각각의 `move()`를 일일이 수정해야할 뿐더러, 같은 Method를 여러 Class에서 똑같이 정의하고 있으므로 **Method의 중복이 발생**하고 있습니다.

## Strategy Pattern 구현

### 먼저 전략을 생성하는 방법입니다.

현재 운송 수단은 Track을 따라 움직이던가, Road를 따라 움직이던가 2가지의 방식이 있습니다. 즉, 움직이는 두 방식에 대해 Strategy Class를 생성하도록 합니다. (TrackStrategy, RoadStrategy)

그리고 두 Class는 `move()`Method를 구현하여, 어떤 경로로 움직이는지에 대해 구현합니다.

또 두 전략 Class를 Encapsulation 하기 위해 MovableStrategy Interface를 생성합니다. 이렇게 Encapsulation을 하는 이유는 운송수단에대한 전략 뿐만 아니라, 다른 전략(ex, 주유 방식 등)이 추가적으로 확장되는 경우를 고려한 설계입니다.

![Strategy Pattern 2](https://nephelai.github.io/images/posts/strategy_pattern_2.jpg)

```java
public interface MovableStrategy {
    public void move();
}
```

```java
public class TrackStrategy implements MovableStrategy {
    public void move() {
        System.out.println("Using Track...");
    }
}
```

```java
public class RoadStrategy implements MovableStrategy {
    public void move() {
        System.out.println("Using Road...");
    }
}
```

### 운송 수단에 대한 Class를 정의합니다.

Train과 Bus 같은 운송 수단은 move() Method를 통해 움직일 수 있습니다. 하지만 이동 방식을 직접  Method로 구현하지 않고, 어떻게 움직일 것인지에 대한 전략을 설정하여, 그 전략의 움직임 방식을 사용하여 움직이도록 구현합니다.

즉, Strategy를 설정하는 Method인 setMovableStrategy()가 존재합니다.

![Strategy Pattern 3](https://nephelai.github.io/images/posts/strategy_pattern_3.jpg)

```java
public class Moving {
    private MovableStrategy movableStrategy;
    
    public void move() {
        movableStrategy.move();
    }
    
    public void setMovableStrategy(MovableStrategy movableStrategy) {
        this.movableStrategy = movableStrategy;
    }
}
```

``` java
public class Bus extends Moving { ... }
```

```java
public class Train extends Moving{ ... }
```

### Train과 Bus 객체를 사용하는 Client를 구현합니다

Train과 Bus의 객체를 생성한 후에 각 운송 수단이 어떤 방식으로 움직이는지 설정하기 위해서 setMovableStrategy() 를 호출합니다.

``` java
public class Client {
    public static void main(String args[]) {
        Moving train = new Train();
        Moving bus = new Bus();
        
        train.setMovableStrategy(new TrackStrategy());
        bus.setMovableStrategy(new RoadStrategy());
        
        train.move();
        bus.move();
        
        // 선로를 따라 움직이는 bus 개발 시
        bus.setMovableStrategy(new TrackStrategy());
        bus.move();
    }
}
```

위의 예시와 같이 쉽게 유지 보수가 가능하다는 것을 볼 수 있습니다.























## 출처

[디자인패턴 - 전략 패턴](https://victorydntmd.tistory.com/292?category=719467)