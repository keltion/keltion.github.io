---
title: "[heap] Find Median from Data Stream"
layout: post
subtitle: heap
categories: codingtest
tags: heap
comments: false
---

# leetcode 295
MedianFinder 클래스를 구현하는 문제입니다. 이 클래스는 데이터 스트림에 정수를 추가하는 addNum함수와
중간값을 리턴하는 findMedian함수를 갖습니다. 중간값만 알면 되기에 중간값 이외의 값들에 정렬이 필요없다는 점을 감안하여
두개의 heap을 이용하여 문제를 풀었습니다. 
  
## complexity
- Time complexity: 
    - addNum : O(logn)
    - findMedian : O(1)
- Space complexity : O(n)  
  
```cpp
bool Compare(int lhs, int rhs){
    return lhs > rhs;
}

class MedianFinder {
public:
    MedianFinder() {
        
    }
    
    void addNum(int num) {
        if(max_heap_.size() == 0 || max_heap_[0] > num) {
            max_heap_.emplace_back(num);
            push_heap(max_heap_.begin(), max_heap_.end());
        }
        else {
            min_heap_.emplace_back(num);
            push_heap(min_heap_.begin(), min_heap_.end(), Compare);
        }
        if(max_heap_.size() == min_heap_.size() + 2) {
            pop_heap(max_heap_.begin(), max_heap_.end());
            int data = max_heap_.back();
            max_heap_.pop_back();
            
            min_heap_.emplace_back(data);
            push_heap(min_heap_.begin(), min_heap_.end(), Compare);            
        }
        else if(min_heap_.size() == max_heap_.size() + 2) {
            pop_heap(min_heap_.begin(), min_heap_.end(), Compare);
            int data = min_heap_.back();
            min_heap_.pop_back();
            
            max_heap_.emplace_back(data);
            push_heap(max_heap_.begin(), max_heap_.end());     
        }
    }
    
    double findMedian() {
        if((max_heap_.size() + min_heap_.size()) % 2 == 1) {
            return max_heap_.size() > min_heap_.size() ? max_heap_[0] : min_heap_[0];
        }
        else {
            return (max_heap_[0] + min_heap_[0]) / 2.0; 
        }
    }
private:
    vector<int> max_heap_;
    vector<int> min_heap_;
};

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder* obj = new MedianFinder();
 * obj->addNum(num);
 * double param_2 = obj->findMedian();
 */
```

 ## 회고
 max_heap_[0] > num 코드에서 에러가 발생했었다. std::vector<int> max_heap_에 원소가
 없을 때 참조를 할 수 없기 때문이다.