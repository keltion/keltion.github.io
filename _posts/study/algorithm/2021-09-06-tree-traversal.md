---
title: "[Tree] binary tree traversal"
layout: post
subtitle: tree
categories: study
tags: algorithm
comments: false
---

오늘은 binary tree traversal을 구현해보았다. 순회 방법은 노드를 방문하는 순서에 따라 preorder, inorder, postorder로 나뉜다.

```c++
//tree.h
#pragma once

class TreeNode
{
public:
    TreeNode(int value);
    
    int Value;
    TreeNode *left;
    TreeNode *right;
};

void preorder(TreeNode *node);
void inorder(TreeNode *node);
void postorder(TreeNode *node);
````

```c++
//tree.cc
#include <iostream>
#include <memory>

#include "tree.h"

TreeNode::TreeNode(int value): Value(value), left(nullptr), right(nullptr) {}


void preorder(TreeNode *node)
{
    if( (node == nullptr))
    {
        return;
    }

    std::cout << node->Value << " ";
    preorder(node->left);
    preorder(node->right);
    return;
}

void inorder(TreeNode *node)
{
    if(node == nullptr)
    {
        return;
    }
    
    inorder(node->left);
    std::cout << node->Value << " ";
    inorder(node->right);
    return;
}

void postorder(TreeNode *node)
{
    if(node == nullptr)
    {
        return;
    }

    postorder(node->left);
    postorder(node->right);
    std::cout << node->Value << " ";
    return;
}
```

```c++
#include <iostream>
#include <memory>

#include "tree.h"


int main()
{
    // 노드 생성
    TreeNode *node1 = new TreeNode(1);
    TreeNode *node2 = new TreeNode(2);
    TreeNode *node3 = new TreeNode(3);
    TreeNode *node4 = new TreeNode(4);
    TreeNode *node5 = new TreeNode(5);
    TreeNode *node6 = new TreeNode(6);
    TreeNode *node7 = new TreeNode(7);

    // 트리 구성
    node1->left= node2;
    node1->right= node3;
    node2->left= node4;
    node2->right= node5;
    node3->left= node6;
    node3->right= node7;


    std::cout << "preorder ==========" << std::endl;
    preorder(node1);
    std::cout << std::endl;

    std::cout << "inorder ==========" << std::endl;
    inorder(node1);
    std::cout << std::endl;

    std::cout << "postorder ==========" << std::endl;
    postorder(node1);
    std::cout << std::endl;
    
    delete node1;
    delete node2;
    delete node3;
    delete node4;
    delete node5;
    delete node6;
    delete node7; 
}
```