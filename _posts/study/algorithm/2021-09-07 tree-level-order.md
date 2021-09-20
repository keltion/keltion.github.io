level order

FIFO queue로 볼 수 있음
```cpp
void levelorder(TreeNode *node)
{
    std::queue<TreeNode*> nodes;
    nodes.emplace(node);
    while(nodes.size()){
        TreeNode* crnt_node = nodes.front();
        std::cout << crnt_node->Value << " ";
        nodes.pop();
        if((crnt_node->left) != nullptr)
            nodes.emplace(crnt_node->left);
        if((crnt_node->right) != nullptr)
            nodes.emplace(crnt_node->right);
    }

}
```