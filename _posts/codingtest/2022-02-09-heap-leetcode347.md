---
title: "[graph] Clone Graph"
layout: post
subtitle: heap
categories: codingtest
tags: heap
comments: false
---
# leetcode 347
빈도가 높은 숫자들을 k개 골라 출력하는 문제입니다. heap을 이용하여 풀이하였습니다.  
 
## complexity
- Time complexity : O(n)
- Space complexity : O(n)  
  
```cpp
bool compare(std::pair<int,int> lhs, std::pair<int, int> rhs ) {
    return lhs.first > rhs.first;
}

class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        std::unordered_map<int, int> map;
        std::vector<std::pair<int,int>> vec;
        std::vector<int> sol;
        
        for(int& num : nums) {
            map[num]++;
        }
        
        for(auto& e : map) {
            vec.emplace_back(e.second, e.first);
            std::push_heap(vec.begin(), vec.end(), compare);
            if(vec.size() > k) {
                std::pop_heap(vec.begin(),vec.end(), compare);
                vec.pop_back();
            }
        }
        
        for(auto& i: vec) {
            sol.emplace_back(i.second);
        }

        return sol;
    }
};
```

## 회고
1. Compare 관련하여 정리
2. heap을 만드는데 걸리는 시간이 왜 O(n)일지 공부해보기
3. std::make_heap 내부 공부해보기
4. std::make_heap으로 컨테이너를 heap으로 만들지 않았다면 Compare를 pop_heap할 때 또 넣어서
   pop의 기준을 정해줘야함 