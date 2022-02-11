---
title: "[greedy] Find Median from Data Stream"
layout: post
subtitle: greedy
categories: codingtest
tags: greedy
comments: false
---

# leetcode 1578
문자열이 제공되고 문자열을 이루고 있는 문자를 지울 때 발생하는 cost가 주어집니다. 중복되는 문자를 하나만 남기고 제거하되
cost를 최소로 하여 제거해야 하는 문제입니다.
  
## complexity
- time complexity : O(n)
- space complexity : O(1)  
  
```cpp
class Solution {
public:
    int minCost(string colors, vector<int>& neededTime) {
        char ch = 0;
        int max = 0;
        int cost = 0;

        for(int i = 0; i < colors.length(); i++) {
            if(ch == colors[i]) {
                if(max < neededTime[i]) {
                    cost += max;
                    max = neededTime[i];
                }
                else {
                    cost += neededTime[i];
                }
            }
            else {
                max = neededTime[i];
            }
            ch = colors[i];    
        }
        return cost;
    }
};

## 회고
없음