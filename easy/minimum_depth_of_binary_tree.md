### 题目
[二叉树的最小深度](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/)

### 思路
和二叉树的最大深度不同的是，如果是左节点没有的话，他会取右节点的值，所以需要改良一下。

### 代码
```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int minDepth(TreeNode* root) {
        if(root == nullptr){
            return 0;
        }
        int res = INT_MAX;
        if(root->left){
            res = min(res, minDepth(root->left));
        }
        if(root->right){
            res = min(res, minDepth(root->right));
        }
        return res == INT_MAX ? 1 : res + 1;
    }
};
```