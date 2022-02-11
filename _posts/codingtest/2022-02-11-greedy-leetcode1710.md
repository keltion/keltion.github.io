---
title: "[greedy] Maximum Units on a Truck"
layout: post
subtitle: greedy
categories: codingtest
tags: greedy
comments: false
---

# leetcode 1710
트럭에 적재할 수 있는 상자의 개수가 주어지고, 상자의 유형과 각 상자유형 안에 들어있는 유닛의 수가 2D vector로 제공됩니다.
트럭에 최대한 유닛이 많도록 상자를 적재하는 방법을 찾는 문제입니다. vector를 유닛의 수로 내림차순 정렬한 후 vector를 순회하며
유닛의 수가 많은 상자유형부터 적재하도록 하였습니다.
  
## complexity
- time complexity : O(nlogn)
- space complexity : O(1)

```cpp 
class Solution {
public:
    int maximumUnits(vector<vector<int>>& boxTypes, int truckSize) {
        int unit = 0;
        std::sort(boxTypes.begin(), boxTypes.end(),
                  [](vector<int>& lhs, vector<int>& rhs) {
                      return lhs[1] > rhs[1];
                  });

        for(auto& box: boxTypes) {
            if(truckSize == 0) {
                break;
            }
            if(box[0] >= truckSize) {
                unit += truckSize * box[1];
                truckSize = 0;
            } 
            else {
                unit += box[0] * box[1];
                truckSize -= box[0];
            }              
        }
        return unit;
    }
};
```

## 회고
없음