## 101  Symmetric Tree
[题目地址](https://leetcode.com/problems/symmetric-tree/description/)
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

//判断树是否镜像
//使用递归

//递归辅助函数，单独拉出来
bool isSymmetricHelp(struct TreeNode* left,struct TreeNode* right) {
    //有一个为NULL，要相等必定左右皆为NULL
    //皆不为NULL，如果左右相等，才相等
    if (!left||!right) return left==right;
    if (left->val!=right->val) return false;


    return isSymmetricHelp(left->left,right->right)&&isSymmetricHelp(left->right,right->left);
}


bool isSymmetric(struct TreeNode* root) {
    if (root==NULL) return true;
    else {
        return isSymmetricHelp(root->left,root->right);
    }
}
```