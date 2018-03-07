## 110  Balanced Binary Tree
[题目地址](https://leetcode.com/problems/balanced-binary-tree/description/)
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

//验证是否为平衡二叉树
//平衡二叉树（AVL树）：任意节点的两棵子树之间的高度差小于等于1

//二叉树的高度：即为树的最大深度
int maxDepth(struct TreeNode* root) {
    if (root==NULL) return 0;
    int L=maxDepth(root->left);
    int R=maxDepth(root->right);
    int M=(L>R)?L:R;

    return M+1;
}

//使用递归
bool isBalanced(struct TreeNode* root) {
    if (root==NULL) return true;
    int maxLeft=maxDepth(root->left);
    int maxRight=maxDepth(root->right);

    return abs(maxLeft-maxRight)<=1&&isBalanced(root->left)&&isBalanced(root->right);
}
```