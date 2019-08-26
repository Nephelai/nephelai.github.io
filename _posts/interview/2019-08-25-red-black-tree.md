---
layout: post
title: Red Black Tree
tags: [dataStructure]
categories: [interview]

---

## Concept

Skewed Tree를 제거하기 위해서 만들어 졌습니다.

기본적으로 일반적인 Binary Search Tree 이지만, Balanced Tree로 만들기 위해 특정 조건이 있습니다.

1. 모든 Node는 **Black** or **Red**입니다.
2. Root Property : Root Node은 **Black**입니다.
3. External Property : 모든 External Node는 **Black**입니다.
4. Internal Property : Red Node의 자식은 **Black**입니다. (No Double Red)
5. Depth Property : 모든 Leaf Node까지의 Black Node의 수는 같다.

## Insert

**Insert 되는 Node는 항상 Red입니다.**

위의 조건에 의해 3번 조건(Internal Property)를 위반하는 경우가 발생합니다. 이를 2가지의 알고리즘으로 인해 해결할 수 있습니다.

2가지의 경우는 부모의 형제(즉, 자신의 Parent's Sibling)을 보고 확인할 수 있습니다.

### Restructuring

부모의 형제의 노드가 **Black**인 경우 동작합니다.

Restructuring의 경우 다른 Sub-Tree에 영향을 미치지 않기 때문에 한번의 Restructuring만 이루어지면 됩니다.

### Recoloring

부모의 형제의 노드가 **Red**인 경우 동작합니다.

Recoloring의 경우 다른 Sub-Tree에 영향을 미칠 수 있습니다. 따라서 Recoloring의 시간복잡도는 최악의 경우 $O(log~n)$이 됩니다.

## Delete

## Performance

모든 Node는 4번 조건에 의해 $log~n$에 찾을 수 있습니다. 즉, Tree의 Height가 $log~n$에 바운드됩니다. 현재 고안된 Binary Search Tree 중 가장 Performance가 좋다고 하며, STL의 Map도 Red-Black Tree를 이용해 구현되어 있습니다.

## 출처

[Zedd0202-알고리즘) Red-Black Tree](https://zeddios.tistory.com/237)