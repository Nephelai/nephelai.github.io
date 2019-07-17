---
layout: post
title: Build Automation, 빌드 자동화
tags: [build]
categories: [InternshipLine]
---

## 빌드 자동화란?

프로젝트에서 개발을 하기전에 보통 Local 개발 환경 세팅을 합니다. IDE 를 설정하고 나서 가장 먼저 하는 일은 build 환경 설정입니다. 정적 타이핑 언어는 보통 code를 작성하면 compile을 하여 object 파일을 생성하고, linking이라는 작업을 통해서 실행 파일 또는 java의 jar와 같은 library 파일을 생성합니다. 언어마다 차이가 있지만 이런 작업들을 자동화하는 작업을 build 자동화라고 합니다.

build Automation Tool은 언어마다 다양합니다. C나 C++의 경우는 보통 Makefile을 이용하고, JAVA는 Ant, Maven를 Scala는 sbt를 주로 사용합니다. 그리고 여러가지 언어 빌드 환경을 구성할 수 있는 Gradle도 있습니다.

### Gradle 장점

#### 간결함

Gradle은 기본적으로 Groovy라는 언어를 활용하여 DSL(Domain Specific Language, 도메인 특화 언어)을 Script로 사용합니다. 일단 xml을 사용하지 않기 때문에 장황하지 않으며, groovy 언어 기반이라 변수 선언, if-else, for 등의 로직 구현이 가능합니다.

Build Script를 만드는 것과 같이 '한정된 주제' 내에서 '반복적인 일'을 하는 상황에서는 일반적인 programming 언어보다 DSL을 만들어 제공하는 것이 나을 수 있습니다.

#### 문서화

gradle의 장점은 문서화가 매우 잘 되어있다는 것입니다. gradle에는 plugin이 굉장히 많기 때문에 모든 문서를 정독하는 것은 불필요합니다. 하지만 이를 제외한 문서가 잘 정리되어 있어서 그냥 읽기만 해도 됩니다.

#### 속도

Gradle은 성능 향상을 위해서 다양한 기능을 제공합니다. gradle은 성능 향상을 위해서 증분 빌드, 작업 결과 캐싱, 증분 하위 작업, 데몬 프로세스 재사용, 병렬 실행, 병렬 다운로드 등의 기능을 지원한다.

## 출처

[디지털 세상을 만드는 아날로거](https://medium.com/@goinhacker/운영-자동화-1-빌드-자동화-by-gradle-7630c0993d09)