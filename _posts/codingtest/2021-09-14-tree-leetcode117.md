---
title: "[Tree] Next Right Pointer"
layout: post
subtitle: tree
categories: codingtest
tags: tree
comments: false
---

# leetcod 117
어떤 노드의 같은 레벨에 있는 오른쪽 노드를 가리키야한다. 즉 앞서 구현해본 levelorder를 이용하면 쉽게 구현할 수 있다.
1. pre_node를 만들고 현재 노드를 가리키도록 하고 큐에 들어있는 현재 노드를 pop한다. 
2. 현재 노드를 큐에 제일 먼저 들어온 노드로 업데이트 해준 뒤 pre_node의 next가 현재 노드를 가리키도록 한다.
근데 여기서 문제가 있다. 같은 레벨에서 가장 오른쪽에 있는 노드가 현재노드일 때는 위의 프로세스를 적용해서는 안된다. 따라서
위 프로세스가 진행되기 전에 queue의 사이즈(각 레벨에 있는 노드의 수)를 계산하고 그 사이즈만큼만 반복하게 하여 예외사항을
처리해줘야한다.

level order

FIFO queue로 볼 수 있음
```cpp
class Solution {
public:
    Node* connect(Node* root) {
        if(!root)
            return root;
        std::queue<Node*> nodes;
        Node* crnt_node;
        Node* pre_node;

        nodes.emplace(root);
        while(nodes.size()) {
            int n = nodes.size();
            pre_node = nullptr;
            while(n--) {
                crnt_node = nodes.front();
                if(pre_node)
                    pre_node -> next = crnt_node;
                pre_node = crnt_node;
                nodes.pop();

                if(crnt_node->left)
                    nodes.emplace(crnt_node->left);
                if(crnt_node->right)
                    nodes.emplace(crnt_node->right);
            }
        }
        return root;
    }
};
```