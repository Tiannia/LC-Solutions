### 题目
[打家劫舍](https://leetcode-cn.com/problems/house-robber-iii/)

### 思路
对于当前节点：只有选与不选
1. 选：我的左右子节点都只能不选。
2. 不选：我可以从左节点选，也可以不选，右节点同理。

可以想到自底向上的树形dp
对于不选，我可以取max(左子选，左子不选) + max(右子选，右子不选)

最后答案就是根节点的选与不选的较大值了。

### 代码
```c++
struct SubtreeStatus {
    int selected;
    int notSelected;
};

class Solution {
public:
    SubtreeStatus dfs(TreeNode* node) {
        if (!node) {
            return {0, 0};
        }
        auto l = dfs(node->left);
        auto r = dfs(node->right);
        int selected = node->val + l.notSelected + r.notSelected;
        int notSelected = max(l.selected, l.notSelected) + max(r.selected, r.notSelected);
        return {selected, notSelected};
    }

    int rob(TreeNode* root) {
        auto rootStatus = dfs(root);
        return max(rootStatus.selected, rootStatus.notSelected);
    }
}; 
```