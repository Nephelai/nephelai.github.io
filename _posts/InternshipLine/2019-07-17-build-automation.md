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

Gradle은 성능 향상을 위해서 다양한 기능을 제공합니다. gradle은 성능 향상을 위해서 증분 빌드, 작업 결과 캐싱, 증분 하위 작업, 데몬 프로세스 재사용, 병렬 실행, 병렬 다운로드 등의 기능을 지원합니다.

#### Multi Project

Gradle은 멀티 프로젝트 구성이 가능합니다. 하나의 repository 내에 여러 개의 하위 project를 구성할 수 있습니다. 하나의 베포 파일(ex. jar)을 만들기 위해서 별도의 project를 만들고 repository를 구성할 필요가 없습니다.

또한 상위 project의 의존성(dependency) 및 설정을 하위 project에서 상속받아 사용할 수 있기 때문에 중복 설정이 불필요합니다.

#### 유연성 및 확장성

Groovy 기반 Scripting을 통해 다양한 기능을 Script 안에 직접 구현할 수 있습니다. 예를 들어 System Property에서 현재 build를 사용하고 있는 사용자의 ID나 OS Version 정보를 가져와서 Artifact의 Version 정보로 사용될 수 있습니다.

### Gradle 설정

`gradle init` 을 통해서 gradle을 생성

**Gradle Wrapper 를 사용하는 목적**은 이미 존재하는 project를 새로운 환경에 설치할 때, 별도의 설치나 설정 과정없이 곧바로 build할 수 있게 하기 위해서 입니다. Java나 Gradle도 설치할 필요가 없으므로 Wrapper 사용이 편합니다.

* gradlew file : Unix 용 실행 Script, gradle로 compile이나 build 등을 할때, `> gradle build` 대신 `>./gradlew build`를 이용합니다.
* gradlew.bat file : Window 용 실행 Script
* gradle/wrapper/gradle-wrapper.jar file : Wrapper 파일, 이 파일 때문에 local 환경의 영향을 받지 않습니다.
* gradle/wrapper/gradle-wrapper.properties file : Gradle Wrapper 설정 파일, 이 파일의 wrapper 버전 등을 변경하면 task 실행 시, 자동으로 새로운 wrapper 파일을 local cache에 download 합니다.
* build.gradle file : dependency나 plugin 설정 등을 위한 script 파일
* settings.gradle file : project의 구성 정보를 기록하는 파일, 어떤 하위 project들이 어떤 관계로 구성되어 있는지를 기술합니다. gradle은 이 파일에 기술된대로 project를 구성합니다.

#### build.gradle depedencies 설정

설정된 repository에서 가져올 artifact를 설정합니다.

``` groovy
dependencies {
    // local jar 파일의 의존성 설정
    compile fileTree(dir: 'libs', include: '*.jar')
    
    // local project 간의 의존성 설정
    compile project(':shared')
    
    // compile 시간에 의존성을 받아옴
    compile 'com.google.guava', name: 'guava:2.3.0'
    
    // Test 시에만 의존성을 받아옴
    // minor 버전을 '+'로 설정해서 항상 4점대 최신 버전을 사용
    testCompile group: 'junit', name: 'junit', version: '4.+'
    
    // compile 할때는 사용하고, artifact를 만들때는 포함하지 않음
    compileOnly 'org.projectlombok:lombok:1.16.18'
    
    // 실행할 때 의존성을 받아옴(기본적으로 compile을 모두 포함)
    runtime('org.hibernate:hibernate:3.0.5')
}
```

## Gradle Vs Maven

기본적으로 gradle이 최신에 나온 만큼 사용성, 성능 등 비교적 뛰어난 spec을 지니고 있습니다.

### Gradle 이 Maven 보다 좋은 점

Build라는 동적인 요소를 XML로 정의하기에는 어려운 부분이 많습니다.

* Maven은 XML 바탕으로 설정 내용이 길어지고 가독성이 떨어집니다.
* dependency 관계가 복잡한 project 설정하기에 부적절합니다.
* 상속 구조를 이용한 멀티 모듈 구현이 어렵습니다.
* 특정 설정을 소수 모듈에서 공유하기 위해서 부모 project를 생성하여 상속하게 하여야 합니다.

Gradle은 Groovy를 사용하기 때문에, 동적인 build는 groovy script로 plugin을 호출하거나 직접 code를 작성하면 됩니다.

* Configuration Injection 방식을 사용해서 공통 모듈을 상속해서 사용하는 단점을 보완했습니다.
* 설정 주입 시 project 조건을 체크할 수 있어서 project 별로 주입되는 설정을 다르게 할 수 있습니다.

**전체적으로 Maven 보다 gradle이 좋다는 것은 Performance Gragh 보면 쉽게 알 수 있습니다**





## 출처

[디지털 세상을 만드는 아날로거](https://medium.com/@goinhacker/운영-자동화-1-빌드-자동화-by-gradle-7630c0993d09)