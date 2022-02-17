---
title: "[greedy] Gas Station"
layout: post
subtitle: greedy
categories: codingtest
tags: greedy
comments: false
---

# leetcode 134
주요소에서 채울 수 있는 가스양과 다음 주요소로 가기 위한 cost가 주어집니다. 모든 주요소를 순회할 수 있는지, 있다면 출발 주요소의 인덱스를 반환해야 하는 문제입니다.
gas_station[] = {5, 1, 2, 3}, cost = {1, 4, 4, 2}라고 가정할때, gas_station[0]에서 출발하면 gas_station[2]까지 밖에 가지 못합니다. 이 때,
gas_station[1], gas_station[2]에서 출발해도 모든 주유소를 순회할 수 없다는 것을 아는것이 중요합니다. 이전 gas_station에서의 얻은 값이 결국 결과에 약간이라도 영향을 미치기 때문입니다.
이 아이디어를 가지고 문제를 푼다면 시간복잡도를 줄일 수 있습니다.
  
## complexity
- time complexity : O(n)
- space complexity : O(1)
  
```cpp
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int remain = 0;
        int sol = -1;
        int temp = 0;

        for(int i = 0; i < gas.size(); i++) {
            int remain = 0;
            for(int j = i; j < i + gas.size(); j++) {
                remain += gas[j % gas.size()];
                if((remain = remain - cost[j % gas.size()]) < 0) {
                    temp = j % gas.size();
                    if(temp < i) return -1;
                    break;
                }
                if(j == i + gas.size() -1) {
                    sol = i;
                }
            }
            i = temp;
            if(sol >= 0) {
                return sol;
            }
        }
        return -1;
    }
};
```

## 회고
- 순회에 실패했다면 실패지점의 주요소의 다음 주요소부터 다시 순회를 시작해야한다. 따라서 내부 for loop안에서 i를 갱신해주어야 했다. 갱신을 한 줄 알았는데 안했었다.. 이것때문에 시간이 오래걸렸다. 