---
layout: post
title: What is Callback Function
tags: [nodejs]
categories: [javascript]
---

일반적인 개발에서는 개발자가 System에 필요한 시점에 어떤 특정 기능을 호출(Call)하는 방식으로 많이 사용됩니다. API를 제공 받아서 API가 필요한 시점에 맞게 호출하는 방식입니다. 이 방법이 일반적인 프로그래밍 방법입니다.

하지만 이러한 함수의 호출을 System에 맡겨야 하는 상황이 발생하기도 합니다. 사용자가 호출 시점을 결정하는 것이 아니라 System 입장에서 필요한 타이밍에 호출이 필요한 상황인 것입니다. 예를 들어 특정 이벤트가 발생할 때, 이를 사용자에게 알려준다고 가정해 봅시다.

특정 이벤트가 발생하기 전에는 program 입장에서 이벤트의 정보를 사용자에게 알려줄 수도 없습니다. (아에 무슨 일이 일어났는지 모르기 때문에) 그렇다고 개발자가 개발시에 사용자에게 알려줄 이벤트가 언제 발생할지도 모릅니다. 이런 경우 이벤트 발생시 data를 전달하게끔 Callback을 이용하여 구현을 해놓습니다.

이 함수는 **다른 함수의 매개변수로 호출될 함수를 전달하고, 특정 이벤트가 발생하고 나서 매개변수로 호출된 함수가 다시 호출되는 것**을 의미합니다.

즉, 일반적인 programming에서 함수를 호출하는 방식은 호출자가 피호출자를 Call하지만, Callback 함수는 피호출자가 호출자를 Call하게 됩니다.

programming에서 callback은 다른 code의 인수로써 넘겨주는 실행 가능한 code를 말합니다. callback을 넘겨받는 code는 이 callback을 필요에 따라 즉시 실행할 수도 있고, 아니면 나중에 실행할 수 있습니다. <위키피디아>

1. passed as an argument to another function : 다른 함수의 인자로써 이용되는 함수
2. is invoked after some kind of event : 어떤 이벤트에 의해 호출되어지는 함수



## 출처

[구스의 엔지니어 세상](https://guslabview.tistory.com/214)