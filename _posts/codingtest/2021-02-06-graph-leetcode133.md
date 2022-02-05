---
title: "[graph] Shortest Path in Binary Matrix"
layout: post
subtitle: graph
categories: codingtest
tags: graph
comments: false
---
# leetcode 1091
graph를 Deep Copy하는 문제입니다. 현재 생성된 노드가 이전에 생성된 노드와 연결이 필요한 경우 이전에 생성된 노드에 대한 주소정보가 없어 이를 처리하는 것이 까다로웠습니다. 
이를 해결해주기 위해서 "mapping"을 사용했습니다. 기존에 자료구조 set을 이용하여 방문노드를 표시하였는데 이번에는 map을 사용하여 위의 문제를 해결하였습니다.  

## complexity
- Time complexity : O(v+e)
- Space complexity : O(n)
```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> neighbors;
    Node() {
        val = 0;
        neighbors = vector<Node*>();
    }
    Node(int _val) {
        val = _val;
        neighbors = vector<Node*>();
    }
    Node(int _val, vector<Node*> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
};
*/

class Solution {
public:
    Node* cloneGraph(Node* node) {
        if(node == nullptr) {
            return nullptr;
        }
        return dfs(node);
    }
    Node* dfs(Node* node) {
        Node* head = new Node(1);
        unordered_map<Node*, Node*> seen;
        stack<Node*> st;
        
        st.emplace(node);
        seen.emplace(node, head);
        
        while(!st.empty()) {
            Node* curr_rnode = nullptr;
            Node* curr_wnode = nullptr;
            curr_rnode = st.top();
            curr_wnode = seen[curr_rnode];
            st.pop();
            for(auto* neighbor : curr_rnode->neighbors) {
                if(seen.find(neighbor) != seen.end()) {
                    (curr_wnode->neighbors).emplace_back(seen[neighbor]);
                }
                else {
                    Node* new_node = new Node(neighbor->val);
                    (curr_wnode->neighbors).emplace_back(new_node);
                    seen.emplace(neighbor, new_node);
                    st.emplace(neighbor);
                }
            }
        }
        return head;
    }
};
```

## 회고
1. 이후에 사용할 포인터를 nullptr로 초기화하지 않아서 발생하는 에러가 많았다.
2. 노드 방문 표시시점을 어떤 기준으로 잡을지 문제에 따라 신중히 결정해야겠다.