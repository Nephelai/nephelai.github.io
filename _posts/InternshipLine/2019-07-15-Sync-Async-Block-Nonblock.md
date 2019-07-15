---
layout: post
title: Synchronous Vs Asynchronous Vs Blocking Vs NonBlocking 
tags: [OS]
categories: [InternshipLine]
---

Synchronous Vs Asynchronous Vs Blocking Vs NonBlocking의 관계를 다음의 그림으로 알 수 있습니다.

![SABN](https://nephelai.github.io/images/posts/developerWorks_Matrix.jpg)

이 중 쉽게 알 수 있는 부분이 2가지 있습니다.

## Synchronous & Blocking

호출 시 다른 작업을 하지 못하고 반환이 이루어져야 다음 작업이 진행됩니다. 

예를 들어 file.read(), file.write() 등이 있습니다.

## Asynchronous & Non-Blocking

이 방법은 호출 후 바로 반환을 통해 그 동안 다른 일을 수행할 수 있는 구조입니다. 그 후 Callback을 통해 작업 완료를 알려줍니다.

예를 들어 대표적으로 Node.js 가 이와 같은 방식입니다.



## Blocking / Non-Blocking

이 2가지의 차이는 return의 동작 시간에 있습니다.

즉, 바로 return이 오는 경우는 Non-Blocking 이며, return이 늦어지는 경우 다른 작업을 할 수 없으므로 Blocking이 됩니다.

## Synchronous / Asynchronous

이 2가지의 차이는 누가 작업을 하는 가에 있습니다.

어떤 작업을 하는 함수를 호출하는 부분을 Caller, 호출 당하는 부분을 Callee라 하면, Caller가 Callee의 작업을 관심을 갖고 보는 경우는 Synchronous, Caller가 Callee에 관심이 없고 나중에 Callback으로 한번에 알려주는 경우를 Asynchronous라고 할 수 있습니다.



위의 차이를 가지고 해결하지 못한 2가지의 부분을 살펴봅시다.



## Synchronous & Non-Blocking

non-blocking이므로 바로 return을 줍니다. 하지만 synchronous 이므로 Caller가 계속해서 지속적으로 완료가 되었는지 확인합니다. 물론 완료가 되었는지 아닌지는 바로 return이 된다는 특성을 지닙니다.

## Asynchronous & Blocking

blocking 이므로 return이 늦어지고, async이므로 Caller는 Callee에 관심을 두지 않고 Callback을 기다립니다. 결국 Caller는 아무일도 하지 않고 그냥 시간을 낭비하는 셈이 됩니다. (물론 다른 일을 하지 않으면서) 결론적으로 보면 아무일도 하지 않기 때문에 매우 비효율적인 방식이라고 생각합니다.

하지만 Nonblocking-Async를 추구하다가 Blocking-Async로 동작하는 경우가 존재합니다. Non-Blocking의 방식을 쓰는 과정에서 하나라도 Blocking 방식으로 동작하는 함수가 존재하면 의도치 않게 Blocking-Async로 동작하게 되어 매우 비효율적으로 동작할 수 있습니다.



## 출처

[뒤태지존의 끄적거림](https://homoefficio.github.io/2017/02/19/Blocking-NonBlocking-Synchronous-Asynchronous/)