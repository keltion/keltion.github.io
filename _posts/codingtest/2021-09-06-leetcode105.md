---
title: "[Tree] Construct Binary Tree from Preorder and Inorder Traversal"
layout: post
subtitle: tree
categories: codingtest
tags: tree
comments: false
---
# leetcode 105
보통 트리는 서브트리로 구성이 되어있다. 즉 큰 문제를 작은 문제로 나누어 접근하는 recursive한 방법을 사용할 수 있으며 이 생각은 이 문제의 핵심 열쇠이기도 하다. preorder를 보고 root node(preorder[0])을 알 수 있고, inorder를 보고 root node를 기준으로 해당 노드들이 어느쪽에 위치한 노드인지 파악할 수 있다. Example 1을 예로 들어보자.
> preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]

root node는 [3](preorder[0])이고 root node를 기준으로 [9]는 left에 [15,20,7]은 right에 위치해 있다는 것을 알 수 있다. 이제 recursive한 방식으로 문제를 다음과 같이 나누어 풀 수 있다.
> 1) preorder = [9] inorder = [9]

> 2) preorder = [20,15,7] inorder = [15,20,7]

이 아이디어를 그대로 코드로 옮겨보자.
```c++
class Solution {
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder){
        int preIndex = 0;
        return helpToBuildTree(preorder, inorder, 0, inorder.size()-1, preIndex);
    }

    TreeNode* helpToBuildTree(vector<int>& preorder, vector<int>& inorder, int is, int ie, int& preIndex) {
        int size = ie-is;
        if(size<0)
        {
            return nullptr;
        }        
        int root = preorder[preIndex++];
        TreeNode *answer = new TreeNode(root);
        
        int rootindex;
        for(rootindex= is; rootindex <= ie ; rootindex++)
        {
            if(root==inorder[rootindex])
            {
                break;
            }
        }

        answer->left =helpToBuildTree(preorder, inorder,  is, rootindex-1, preIndex); 
         answer-> right =helpToBuildTree(preorder, inorder, rootindex+1, ie, preIndex);
        return answer;
    }
};
```

구현에서 시간이 오래걸렸다. 파이썬으로 슬라이싱을 하면 되는것을 c++에서는 함수하나를 만들어서 구현을 해줘야했다. 또 처음에 트리의 recursive한 성질을 파악하지 못하여 헤맸었는데 앞으로는 이를 먼저 생각해봐야겠다. 