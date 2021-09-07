---
title: "[DP] Dynamic Programing이란"
layout: post
subtitle: dp
categories: study
tags: algorithm
comments: false
---
# Dynamic programing
tree 문제를 풀다가 큰 문제를 작은 문제로 나누어 푸는 문제를 접하였고 이전에 공부했던 DP에 대해 다시 정리해야할 필요성을 느꼈다.

Dynamic Programing은 복잡한 문제가 다음 특성을 가질 때 적용할 수 있는 알고리즘 풀이법이다.
1. 복잡한 문제가 더 작은 sub problem으로 나눠지며, sub solution으로 복잡한 문제를 풀 수 있을 때
2. sub problem들이 겹쳤을 때(메모리제이션)

DP는 top-down방식과 bottom-up 방식으로 구현될 수 있다. top-down 방식은 이해하기 쉽지만 call stack의 제한 때문에 보통 bottom-up 방식을 사용한다. DP를 설명하기위해서 피보나치 수열을 예로 드는데 이후에 시간이 날때 작성해보겠다.
