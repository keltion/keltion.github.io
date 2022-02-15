---
title: "[greedy] Non-overlapping Intervals"
layout: post
subtitle: greedy
categories: codingtest
tags: greedy
comments: false
---

# leetcode 435
interval들이 주어지고, 최소한의 interval을 제거하여 interval간에 겹치지 않도록 만드는 문제입니다.
이 문제는 겹치지않는 interval들의 최대개수를 찾으면 풀리는 문제입니다. 백준의 회의실 배정 문제와 비교하여 풀면 좋을 것 같습니다.
    
## complexity
- time complexity : O(nlogn)
- space complexity : O(1)  
  
```cpp
class Solution {
public:
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        int non_overlapping = 0;
        int sol;
        sort(intervals.begin(), intervals.end(),
             [](vector<int>& lhs, vector<int>& rhs) {
                 return lhs[1] < rhs[1]; 
             });

        int before_end_time = intervals[0][0];
        for(auto& interval : intervals) {
            if(before_end_time <= interval[0]) {
                non_overlapping++;
                before_end_time = interval[1];
            }
        }

        sol = intervals.size() - non_overlapping;
        return sol;
    }
};
```