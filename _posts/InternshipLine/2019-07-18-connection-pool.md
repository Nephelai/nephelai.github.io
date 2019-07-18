---
layout: post
title: Connection Pool
tags: [DB]
categories: [InternshipLine]
---

Connection Pool이란, **DB와 미리 connection을 해놓은 객체들을 pool에 저장해 두었다가, Client Request시 connection을 빌려주고 끝나면 connection을 반납하여 pool에 저장하는 방식**을 말합니다.

![ConnectionPool](https://nephelai.github.io/images/posts/connection_pool.jpg)

Client가 DB와 연결이 필요할 때, Connection Pool에서 Connection을 빌려와 DB에 접근하여 Query를 날리고 볼일이 끝나고 사용했던 Connection을 다시 pool에 반납하는 과정입니다.

Connection Pool을 사용하게 되면, 미리 연결을 맺고 있는 Connection들이 있기 때문에 Connection을 맺고 끊는 과정이 불필요합니다. 즉, DB 접근 시 불필요한 작업(Connection 생성, 끊기)이 사라지므로 성능향상을 기대할 수 있습니다.

DB에서 기본적으로 Connection 일정량을 제공합니다. 그런데 User가 많아져서 Connection이 모자를 경우 원활한 서비스가 이루어지지 않습니다. Connection 반납을 기다리기 때문입니다. 따라서 User수에 따라서 connection 수를 조절할 필요가 있습니다. Connection 또한 객체이므로 memory를 차지하기 때문에 무조건 늘리면 좋지 않습니다. 메모리를 많이 차지하여 오히려 성능이 떨어지는 결과를 가져옵니다.

### WAS(Web Application Server)와 DB Server 간의 Issue

WAS에서 DB 서버에 접근을 시작하고 data를 가져오기 까지의 단계에서 가장 비용이 많이 드는 부분은 어디일까?

Java에서 JDBC Diver를 이용한 DB 접근 과정을 생각해봅시다.

1. DB 서버 접속을 위해 JDBC Driver를 Load한다.
2. DB 접속 정보와 DriverManager.getConnection() 을 통해 DB Connection 객체를 얻는다.
3. Connection 객체로부터 Query를 수행하기 위한 PreparedStatement 객체를 받는다.
4. executeQuery를 수행하여 그 결과로 ResultSet 객체를 받아서 data를 처리한다.
5. 처리가 완료되면 처리에 사용된 Resource 들을 close 하여 반환한다.

**이 과정에서 가장 느린 부분은 웹 서버에서 물리적으로 DB서버에 최초로 연결되어 Connection 객체를 생성**하는 부분입니다. 

![ProcessRequest](https://nephelai.github.io/images/posts/process_request.jpg)

웹 Application은 HTTP 요청에 따라 Thread를 생성하게 되고 대부분의 business 로직은 DB서버로 부터 data를 얻게 됩니다. 만약 위와 같은 모든 요청에 대해 DB 접속을 위한 Driver를 Load하고 Connection 객체를 생성하여 연결한다면 물리적으로 DB 서버에 지속적으로 접근해야 합니다.

이런 상황에서 DB Connection 객체를 생성하는 부분에 대한 비용과 대기 시간을 줄이고, Network 연결에 대한 부담을 줄일 수 있는 방법을 **DBCP(Database Connection Pool)**라 불리는 방법입니다.

### 구체적으로...

Connection Pool을 효율적으로 관리하기 위한 값들이 존재합니다.

| Value       | Description                                                  |
| ----------- | ------------------------------------------------------------ |
| maxActive   | 동시에 사용할 수 있는 Max Connection 수                      |
| maxIdle     | Connection Pool에 반납할 때, 최대로 유지될 수 있는 Connection 수 |
| minIdle     | 최소한으로 유지한 Connection 수                              |
| initialSize | 최초로 getConnection()를 통해 Connection Pool에 채워 넣을 Connection 수 |

**maxActive >= initialSize** : 최대 Connection 개수는 초기에 생성할 Connection 수와 같거나 크게 설정해야 합니다.

**maxActive = maxIdle** : maxActive 값과 maxIdle 값은 같은것이 바람직합니다.

```
maxActive = 10;
maxIdle = 5;
```

라고 가정해봅시다.

Connection이 현재 5개를 사용하고 있는 상황에서 Connection 요청이 들어오면 maxActive가 10 이므로 한개의 Connection을 연결하여 제공합니다. 하지만 이후 이 Connection이 반환되면, maxIdle에 의해 Connection을 Pool에 넣지 못하고 Close가 발생합니다. 이와 같이 일부 Connection이 매번 생성했다가 닫는 비용이 발생할 수 있습니다.

#### WAS의 Thread 수와 Connection Pool 수의 관계

WAS에서 설정해야하는 값 중에 성능에 가장 많은 영향을 주는 부분은 Thread와 Connection Pool의 개수라고 할 수 있습니다.

이들 값은 직접적인 memory와 관련이 있기 때문에 많이 사용하면 할 수록 memory를 많이 점유하게 됩니다. 그렇다고 memory를 위해 적게 지정한다면, Server에서는 많은 Request를 처리하지 못하고 대기할 수 밖에 없습니다.

그렇다면 어떻게 관리를 해야할까?

가장 좋은 방법은 Application을 실제 운영할 시스템 환경에서 성능 Test를 진행하는 것입니다. Performance Test를 진행하면서 지금까지 살펴본 DBCP에 대한 원리와 설정을 위한 값들을 고려하여 시스템 환경에 최적화된 값을 찾아내는 것이 좋습니다.

**WAS의 Thread 수는 DB의 Connection Pool의 수보다 많아야 한다.**

모든 Application의 Request가 DB에 접근하는 것이 아니기 때문입니다. 따라서 Thread는 Connection Pool의 개수보다 여유있게 설정하는 것이 좋습니다.

System 환경마다 다르지만, 보통 Connection Pool을 40~50개로 지정하면 Thread는 이보다 10개정도 더 지정하는 것이 바람직합니다. 하지만 최적의 성능을 위해서는 실제 요청이 얼마나 들어오는지 파악하는게 중요하며 가장 좋은 방법은 앞서 말한 것처럼 Performance Test를 통해 최적화된 값을 구하는 것입니다.



## 출처

[victolee 블로그 - Connection Pool](https://victorydntmd.tistory.com/42)

[holaxprogramming - DB Connection Pool 에 대한 이야기](https://www.holaxprogramming.com/2013/01/10/devops-how-to-manage-dbcp/)

[Naver D2 - Commons DBCP 이해하기](https://d2.naver.com/helloworld/5102792)