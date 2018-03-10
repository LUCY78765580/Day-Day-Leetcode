## 104  Maximum Depth of Binary Tree
[题目地址](https://leetcode.com/problems/maximum-depth-of-binary-tree/description/)
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

//求二叉树的最大深度
//使用递归
int maxDepth(struct TreeNode* root) {
    if (root==NULL) return 0;
    int L=maxDepth(root->left);
    int R=maxDepth(root->right);
    int M=(L>R)?L:R;

    return M+1;
}
```