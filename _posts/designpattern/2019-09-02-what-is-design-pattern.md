---
layout: post
title: What is Design Pattern
tags: []
categories: [designpattern]
---

## 디자인 패턴이란?

SoftWare를 설계할 때 특정 맥락에서 자주 발생하는 고질적인 문제들이 또 발생했을 때 재사용할 수 있는 해결책입니다. 간단히 **많은 프로그래머들이 인정한 효율적인 코딩 방법 / 구조 입니다.**

## GoF 디자인 패턴

4명의 프로그래머가 디자인 패턴을 구체화하고 체계화를 하였습니다. 총 23가지의 디자인 패턴을 정리하고 각각의 디자인 패턴을 3가지로 분리하여 구분하였습니다.

### 생성(Creational) 패턴

**객체 생성에 관련된 패턴**으로 객체의 생성과 조합을 캡슐화하여 특정 객체가 생성되거나 변경되어도 프로그램 구조에 영향을 크게 받지 않도록 유연성을 제공합니다.

#### Abstract Factory

구체적인 Class에 의존하지 않고 서로 연관되거나 의존적인 객체들의 조합을 만드는 Interface를 제공하는 패턴

#### Factory Method

객체 생성 처리를 Sub-Class로 분리하여 처리하도록 캡슐화하는 패턴

#### Singleton

전역 변수를 사용하지 않고 객체를 하나만 생성하도록 하며, 생성된 객체를 어디에서든지 참조할 수 있도록 하는 패턴

#### Builder

#### Prototype

### 구조(Structural) 패턴

**클래스나 객체를 조합하여 더 큰 구조를 만드는 패턴**입니다.

#### Composite

여러 개의 객체들로 구성된 복합 객체와 단일 객체를 Client에서 구별 없이 다루게 해주는 패턴

#### Decorator

객체의 결합을 통해 기능을 동적으로 유연하게 확장할 수 있게 해주는 패턴

#### Adapter

#### Bridge

#### Facade

#### Flyweight

#### Proxy

### 행위(Behavioral) 패턴

#### 옵저버(Observer)

한 객체의 상태 변화에 따라 다른 객체의 상태로 연동되도록 일대다(One to Many) 객체 의존 관계를 구성하는 패턴

#### 스테이트(State)

객체의 상태에 따라 객체의 행위 내용을 변경해주는 패턴

#### 스트래티지(Strategy)

행위를 Class로 캡슐화해 동적으로 행위를 자유롭게 바꿀 수 있게 해주는 패턴

#### 템플릿 메서드(Template Method)

어떤 작업을 처리하는 일부분을 Sub-Class로 캡슐화하여 전체 일을 수행하는 구조는 바꾸지 않으며 특정 단계에서 수행하는 내용을 바꾸는 패턴

#### 커맨드(Command)

실행될 기능을 캡슐화를 통해 주어진 여러 기능을 실행할 수 있는 재사용성이 높은 Class를 설계하는 패턴

#### Chain of Responsibility

#### Interpreter

#### Iterator

#### Memento

#### Visitor

## 출처

[개발자를 꿈꾸는 프로그래머 - 디자인 패턴의 정의와 종류](https://jwprogramming.tistory.com/68)

[꿈꾸는 사람 - 디자인 패턴](https://dreamlog.tistory.com/577)

[Design Pattern - 디자인 패턴 종류](https://gmlwjd9405.github.io/2018/07/06/design-pattern.html)