---
title: "[heap] Merge k Sorted Lists"
layout: post
subtitle: heap
categories: codingtest
tags: heap
comments: false
---

# leetcode 23
멤버변수 val값으로 정렬된 k개의 리스트가 주어집니다. k개의 리스트를 하나의 정렬된 리스트로 합쳐 반환하는 문제입니다.
k개의 min_heap을 사용하여 문제를 해결했습니다.

## complexity
- Time complexity : O(nlogk)
- Space complexity : O(k)   
  
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */

bool compare(ListNode* lhs, ListNode* rhs) {
    return lhs->val > rhs->val;
}
class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        int n=0;
        vector<ListNode*> vec;
        ListNode* head = nullptr;
        ListNode* curr = nullptr;
        
        for(auto& node : lists) {
            if(node != nullptr) {
                vec.emplace_back(node);
                std::push_heap(vec.begin(), vec.end(), compare);
            }
        }
        
        while(vec.size() != 0) {
            std::pop_heap(vec.begin(), vec.end(), compare);
            ListNode* top_node=vec.back();
            vec.pop_back();
            if(n++ == 0) {
                curr = head = top_node;
            }
            else {
                curr->next = top_node;
                curr = curr->next;
            }
            if(top_node->next != nullptr) { 
                vec.emplace_back(top_node->next);
                std::push_heap(vec.begin(), vec.end(), compare);
            }
        }
        return head;
    }
};
```  
## 회고 
nullptr이 발생할 수 있는 경우를 생각하며 코드작성하기