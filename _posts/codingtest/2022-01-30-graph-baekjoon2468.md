---
title: "[graph] 안전 영역"
layout: post
subtitle: graph
categories: codingtest
tags: graph
comments: false
---
# leetcode 733
vertex 하나와 새로운 color값이 주어집니다. 이 vertex와 인접한 vertex 중 값이 같은 vertex의 값을
새로운 color값으로 갱신합니다. 또 인접한 vertex와 인접한 vertex들에 대해서도 재귀적으로 진행하여 값을 갱신
하는 문제입니다. (처음 주어진 vertex도 새로운 color값으로 갱신해야함.)

즉 결국은 주어진 좌표를 포함하는 connected components를 찾는 문제라는 것을 알 수 있습니다. BFS, DFS 모두
사용할 수 있으며 이번에는 BFS를 이용하여 풀었습니다.

```cpp

```

## 회고
방문노드인지를 검증하는 조건문에서 segmentation fault가 발생했다. verify()로 갱신된
좌표가 image배열의 범위 내부에 있는지를 먼저 검사를 하고 다른 검사를 진행해야하는데, 거꾸로하여
발생한 에러였다.