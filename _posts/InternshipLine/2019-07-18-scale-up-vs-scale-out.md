---
layout: post
title: Scale Up과 Scale Out
tags: []
categories: [InternshipLine]
---

흔히 Server에서 처리 능력을 향상시키는 방법에는 2가지가 있습니다.

## Scale Up

**서버 그 자체를 증강**하는 것이 의해서 처리 능력을 향상 하는 방법입니다. 수직 Scale 이라고 불리기도 합니다. 전형적으로는 SMP(Symmetric Multi-Processing, 대칭형 다중 처리)에 대해 processor를 추가하는 것이나 processor 그 자체는 고성능 모델로 옮겨놓는 것을 가르킵니다.

빈번한 Update가 발생하여 data의 정합성 유지가 어려운 DB Server에 경우 Scale-Up이 이용됩니다. 즉, 하나의 이미지 DB에 대해서 빈번이 Update가 발생하는 이른바 OLTP(OnLine Transaction Processing, 온라인 트랜젝션 처리)에는 Scale-Up이 적합합니다.

## Scale Out

**접속된 서버의 대수를 늘려** 처리 능력을 향상하는 방법입니다. 수평 Scale이라고도 불립니다. 서버의 가상화 기능을 사용하고 하나의 Case 내에서 가상적으로 복수 서버를 구축해 Scale Out과 동등한 효과를 제공할 수도 있습니다. 이를 가상 Scale Out이라고도 합니다.

각각의 처리는 비교적 단순하지만 다수의 처리를 동시 병렬적으로 실시하지 않으면 안되는 경우에 적합합니다. 또한 갱신 data의 정합성 유지에 대한 요건이 별로 어렵지 않은 경우에 적절합니다. 메일 서버나 게시판 등의 Application 등에 적용할 수 있습니다.

|             | Scale Out                                                    | Scale Up                                                     |
| ----------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 확장성      | 하나의 장비에서 처리하던 일을 여러 장비에 나누어 처리할 수 있도록 설계를 바꾸는 것, 지속적 확장이 가능하다. | 더 빠른 속도의 CPU로 변경하거나, 더 많은 RAM을 추가하는 등의 HW장비의 성능을 높이는 것, 성능 확장에 한계가 있다. |
| Server 비용 | 비교적 저렴한 서버를 사용하므로 일반적으로 비용 부담이 적다. | 성능 증가에 따른 비용 증가폭이 크며, 일반적으로 비용 부담이 크다. |
| 운영 비용   | 대수가 늘어날수록 관리 편의성이 떨어지며, 서버의 거주 비용을 포함한 운영 비용이 증가함. | 관리 편의성이나 운영 비용은 Scale Up에 따라 큰 변화가 없다.  |
| 장애        | Read/Write가 여러 대의 서버에 분산되어 처리됨으로써 장애 발생 시 전면 장애의 가능성이 적다. | 한대의 서버에 부하가 집중되므로 장애 발생 시 장애 영향도가 크다. |
| 주요 기술   | Sharding, Query-off Loading, Queue, In Memory Cache, NoSQL, Object Storage, Distributed Storage | 고성능 CPU, Memory 확장, SSD                                 |

## 출처

[islove8587님의 블로그](https://blog.naver.com/PostView.nhn?blogId=islove8587&logNo=220548900044&proxyReferer=https%3A%2F%2Fwww.google.com%2F)