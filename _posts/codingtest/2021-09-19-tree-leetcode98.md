---
title: "[Tree] Validate Binary Search Tree"
layout: post
subtitle: tree
categories: codingtest
tags: tree
comments: false 
---

# leetcode 98
입력되는 트리가 BST인지를 확인하고 맞다면 True를 아니라면 False를 반환해야하는 문제다.
BST는 이진 트리에 이분하는 규칙을 추가한 트리이다. 이진트리가 다음 규칙을 만족하면 BST가된다.
1. 왼쪽 자식은 언제나 부모보다 작다
2. 오른쪽 자식은 언제나 부모보다 크다


## 예시
![p1](/assets/ct/tree1.png)

## 1. Brute force
이 문제를 Brute force 방식으로 해결해보자. root node인 9를 기준으로 왼쪽 노드들은 모두 9보다 작아야하며, 오른쪽 노드들은 모두 9보다 커야한다. node들을 순회하며 값을 비교하면 O(n)의 time complexity가 발생할 것이다. 이 과정을 다시 모든 노드들에 대해 적용해야하니 O(n<sup>2</sup>)이 된다. 
```cpp
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        TreeNode* crn_node = root;
        if(root==nullptr) {
            return true;
        }
        
        bool left_nodes = helpValid(root->left, root->val, 0);
        bool right_nodes = helpValid(root->right, root->val, 1);
        
        if ( !(left_nodes)|| !(right_nodes)) return false;
            
        bool re1 = isValidBST(root->left);
        bool re2 = isValidBST(root->right);
        return re1 && re2;
    }
    
    bool helpValid(TreeNode * node, int value, bool stat) {
        
        if ( node == nullptr) {
            return true;
        }
        if ( stat==0 && node->val >= value ) {
            return false;
        }
        if ( stat==1 && node->val <= value) {
            return false;
        }
        bool r1 = helpValid(node->left, value, stat);
        bool r2 = helpValid(node->right, value, stat);
        return r1 && r2;
    }
};
```


## 2. time complexity를 줄이는 방법
각 노드를 순회하여 노드값을 검증할때 노드의 값이 존재할 수 있는 범위를 미리 계산하여 비교해준다면 time complexity를 O(n)으로 줄일 수 있을 것이다.
```cpp
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        return helpIsValid(root, nullptr, nullptr);
    }
    
    bool helpIsValid(TreeNode * node, int* min, int* max) {
        
        if (node == nullptr) {
            return true;
        }
        int value = node->val;
        if ( min != nullptr && value <= *min) {
            return false;
        } 
        if ( max != nullptr && value >= *max) {
            return false;
        }
        
        bool lr = helpIsValid( node->left, min, &value );
        bool rr = helpIsValid( node->right, &value, max );
        
        return lr && rr;
        
    }
};
```

## 3.회고
문제에서 노드의 값의 범위를 -2<sup>31</sup>~ 2<sup>31</sup>-1 로 제시하였다. 처음에 min, max를 설정할때 INT_MIN, INT_MAX로 두니 노드의 값이 양 끝값일 상황일 때 기대하는 결과가 나오지 않았다. 따라서 min, max를 int*로 두고 초기 min,max를 nullptr로 설정하여 해결하였다. 