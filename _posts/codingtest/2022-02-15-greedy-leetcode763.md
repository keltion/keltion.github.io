---
title: "[greedy] Partition Labels"
layout: post
subtitle: greedy
categories: codingtest
tags: greedy
comments: false
---

# leetcode 763
문자열이 주어지고 문자열을 이루는 문자가 하나의 영역에 속하도록 분할하는 문제입니다. 단 가능한 많은 분할을 해야합니다.
(이 조건이 없으면 무분할도 답이 될 수 있습니다.) 이 문제는 각각의 문자가 마지막으로 나오는 인덱스를 알고 있다면 쉽게 풀 수 있습니다.

## complexity
- time complexity : O(n)
- space complexity : O(n)
  
```cpp
class Solution {
public:
    vector<int> partitionLabels(string s) {
        unordered_map<char, int> last_idx;
        vector<int> sol;
        for(int i = 0; i < s.length(); i++) {
            last_idx[s[i]] = i; 
        }
        int before_size = 0;
        int max_idx = 0;
        for(int i = 0; i < s.length(); i++) {
            if(max_idx < last_idx[s[i]]) {
                max_idx = last_idx[s[i]];
            }
            if(i == max_idx) {
                sol.emplace_back((i+1) - before_size);
                before_size = (i+1);
            }
        }
        return sol;
    }
};
```

## 회고
- brute-force 방법으로도 풀어보기