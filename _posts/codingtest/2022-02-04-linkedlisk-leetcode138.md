---
title: "[linked list] Copy List with Random Pointer"
layout: post
subtitle: linkedlist
categories: codingtest
tags: linkedlist
comments: false
---
주어진 링크드리스트를 Deep Copy하는 문제입니다. 멤버변수로 data와 next를 가진 평범한 링크드리스트를 Deep Copy하는 것은 어렵지 않습니다. 이런 경우 새 노드를 만들고 주어진 링크드리스트를 순회하며 읽은 data를 카피해주면 쉽게 해결가능하기 때문입니다. 하지만 이 문제에서는 각 노드들은 임의의 노드를 가리키는 random이라는 멤버변수도 갖고 있었기에 난이도가 높았습니다.
  
저는 "mapping"으로 이 문제를 해결하였습니다. 주어진 링크드리스트의 노드를 (X1, X2, X3), Copy한 링크드리스트의 노드를 (x1, x2, x3)이라하고 X1의 random은 X3의 주소라고 가정해보겠습니다. 이 경우 x1이 x3의 주소를 가리키게 해야하는데 이때 X1이 X3을 가리킨다는 정보밖에 존재하지 않습니다. 이때 만약 X3과 x3이 매핑되어 있다면 이 문제를 해결할 수 있을 것입니다.      
  
따라서 링크드리스트를 카피할 때 기존 링크드리스트들의 노드와 새 링크드리스트의 노드를 1대1로 매핑하여 문제를 풀었습니다.      
```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;
    
    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};
*/

class Solution {
public:
    Node* copyRandomList(Node* head) {
        std::unordered_map<Node*, Node*> map;
        Node* dummy = new Node(0);
        
        Node* curr_rnode = head;
        Node* curr_wnode = dummy;

        while(curr_rnode) {
            Node* new_node = new Node(curr_rnode->val);
            curr_wnode->next= new_node;
            map.emplace(curr_rnode, curr_wnode->next);

            curr_rnode = curr_rnode->next;
            curr_wnode = curr_wnode->next;
        }
        curr_rnode = head;
        curr_wnode = dummy->next;
        while(curr_rnode) {
            curr_wnode->random = map[curr_rnode->random];
            
            curr_rnode = curr_rnode->next;
            curr_wnode = curr_wnode->next;
        }
        return dummy->next;
    }
};
```

## 회고
없음