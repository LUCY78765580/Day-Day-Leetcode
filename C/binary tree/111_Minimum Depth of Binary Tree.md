## 104  Minimum Depth of Binary Tree
[题目地址](https://leetcode.com/problems/minimum-depth-of-binary-tree/description/)
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

//二叉树的最小深度，与前面二叉树最大深度略有些不一样
//使用递归

//1、一般思路
int minDepth(struct TreeNode* root) {
    if(root==NULL) return 0;
    if(root->left==NULL && root->right==NULL) {
        return 1;
    }
    else if(root->left && root->right==NULL) {
        return minDepth(root->left) + 1;
    }
    else if(root->right && root->left==NULL) {
        return minDepth(root->right) + 1;
    }

    int left=minDepth(root->left) + 1;
    int right=minDepth(root->right) + 1;
    return left<right ? left:right;

}

//2、递归简洁版本
int minDepth(struct TreeNode* root) {
    if (root==NULL) return 0;
    int L=minDepth(root->left);
    int R=minDepth(root->right);
    int M=(L<R)?L:R;

    return (L==0 || R==0)?L+R+1:M+1;
}
```