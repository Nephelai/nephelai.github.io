---
layout: post
title: Async Programming in NodeJS
tags: [nodejs]
categories: [javascript]
---

JS가 다른 언어들과 구분되는 큰 특징 중의 하나는 바로 Single Thread를 기반으로 하는 비동기 방식의 언어라는 점입니다. 이런 특징에 힘입어 Non-blocking IO를 사용하는 NodeJS의 언어로 사용되면서 최근에는 Server-Side 에서도 큰 인기를 얻고 있습니다. 하지만 이런 구조적 특징에서 오는 단점이 존재합니다. 대표적인 것으로 **연속적 전달 방식(CPS)으로 인한 Callback 지옥**입니다.

Callback 지옥을 해결하기 위한 많은 시도가 있었는데, 그 방법으로 Promise 방법과 Generator 방법입니다. Promise 방법은 Callback 지옥을 해결하기 위한 도구가 아니며, 단지 완화시킬수 있는 방법을 제공합니다. 그리고 ES6에는 비동기 programming을 위한 더 중요한 도구가 Generator입니다.

### generator란?

함수의 실행을 중간에 멈추었다가 필요한 시점에 다시 재개할 수 있습니다. 일종의 Coroutine이라고 볼 수 있지만, 다른 Coroutine과 달리 멈출 때, 돌아갈 위치를 직접 지정할 수 없고, 단순히 호출자에게 제어권을 넘겨줍니다. 그래서 Semi-Coroutine이라고 불립니다.

```javascript
function *myGen() {
    const x = yield 1;       // x = 10
    const y = yield (x + 1); // y = 20
    const z = yield (y + 2); // z = 30
    return x + y + z;
}

const myItr = myGen();
console.log(myitr.next());   // {value:1, done:false}
console.log(myitr.next(10)); // {value:11, done:false}
console.log(myitr.next(20)); // {value:22, done:false}
console.log(myitr.next(30)); // {value:60, done:true}
```

myGen() Generator는 실행될 때, Iterator를 반환합니다. 그리고 Iterator의 next() 가 호출될 때마다 호출되는 곳의 위치를 기억해둔 채로 실행됩니다. 그리고 함수 내부에서 yield를 만날 때마다 기억해둔 위치로 제어권을 넘겨줍니다. 이런식으로

``` next() -> yield -> next() -> yield -> next() ``` 의 흐름이 만들어집니다.

여기서 중요한 점은 next()와 yield가 서로 data를 주고 받을 수 있다는 점입니다. yield keyword 뒤에 값은 next()의 함수의 반환값(정확히 value parameter)으로 전달됩니다. 그리고 반대로 호출자가 Generator에게 값을 전달할 수도 있습니다.

next()를 호출할 때 인수로 값을 지정하면 yield keyword가 있는 대입문에 할당되는 것을 볼 수 있습니다. 이런식으로 서로 data를 주고 받을 수 있습니다.

### Promise

#### Promise는 다음 중 하나의 상태(state)가 됩니다.

* pending : 아직 promise를 수행중인 상태(fulfilled or reject 되기 전)입니다.
* fulfilled : promise가 지켜진 상태입니다.
* rejected : promise가 어떤 이유에서 못 지켜진 상태입니다.
* settled : promise의 여부에 상관없이 일단 결론이 난 상태입니다.



## 출처

[Toast Meetup! - ES6의 제너레이터를 사용한 비동기 프로그래밍](https://meetup.toast.com/posts/73)