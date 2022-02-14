---
title: "[greedy] Two City Scheduling"
layout: post
subtitle: greedy
categories: codingtest
tags: greedy
comments: false
---

# leetcode 1029
2n명의 지원자를 반으로 나눠 city a와 b에 최소한의 비용으로 보내는 방법을 찾는 문제입니다. 각 지원자들마다 a, b에 가기위해 드는 비용이 주어집니다.
  
## complexity
- time complexity : O(nlogn)
- space complexity : O(1)
  
```cpp
class Solution {
public:
    int twoCitySchedCost(vector<vector<int>>& costs) {
        int half_size = costs.size() / 2;
        int size = costs.size();
        int min_cost = 0;
        
        sort(costs.begin(), costs.end(),
             [](vector<int>& lhs, vector<int>& rhs) { 
                 return lhs[1] - lhs[0] > rhs[1] - rhs[0];});
        for(int i = 0; i < half_size; i++) {
            min_cost += costs[i][0];   
        }
        
        for(int i = half_size; i < size; i++) {
            min_cost += costs[i][1];   
        }
        return min_cost;
    }
};
```
## 회고
없음