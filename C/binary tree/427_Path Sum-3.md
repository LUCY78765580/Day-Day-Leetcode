## 427  Path Sum III
[题目地址](https://leetcode.com/problems/path-sum-iii/description/)
<br>
<br>
<br>

```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */

//和为指定值的二叉树节点路径（计算路径条数）
//只要满足自上而下，路径可以是多条
//使用递归

//递归辅助函数，单独拎出来
//每一次递归，sum都要减去一个node->val
//一直到碰到有node->val==sum，说明路径存在，计数加1
int pathSumHelp(struct TreeNode* node,int sum) {
    if (node==NULL) return 0;
    return (node->val==sum?1:0)+pathSumHelp(node->left,sum - node->val)+pathSumHelp(node->right,sum - node->val);
}


int pathSum(struct TreeNode* root, int sum) {
    if (root==NULL) return 0;

    //从root开始，在整个大范围内寻找
    //从之后的left和right开始
    return pathSumHelp(root,sum)+pathSum(root->left,sum)+pathSum(root->right,sum);
}
```