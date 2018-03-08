## 098  Validate Binary Search Tree
[题目地址](https://leetcode.com/problems/validate-binary-search-tree/description/)
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


//验证是否为二叉搜索树BST
//BST特性：1、左节点及以下节点的值比它小；2、右节点及以下节点的值比它大。

/*
//下面这种写法是错的（因为中间也要用到isValidBST,递归到只有一个子节点时候可能就出问题了）
bool isValidBST(struct TreeNode* root) {
    //空树
    if (root==NULL) return true;
    //只有一个节点
    if (root->left==NULL&&root->right==NULL) return true;
    //左子树为空
    if (root->left==NULL) {
        if (root->val < root->right->val&&isValidBST(root->right)) {
            return true;
        }
        return false;
    }
    //右子树为空
    if (root->right==NULL) {
        if (root->val > root->left->val&&isValidBST(root->left)) {
            return true;
        }
        return false;
    }
    //左右子树不为空
    if (root->val > root->left->val && root->val < root->right->val \
        &&isValidBST(root->left)&&isValidBST(root->right)) {
        return true;
    }

    return false;
}
*/

//方法一：使用递归

/*
//1、最原始方式(有bug)
//但是程序没有考虑结点值为INT_MIN的情形
//[ Long_MIN , null , Long_MAX ]这个test case就通不过了
bool ValidBST(struct TreeNode* root,struct TreeNode* min,struct TreeNode* max) {
    if (root==NULL) return true;
    if (root->val > max->val || root->val < min->val) return false;

    return ValidBST(root->left,min,root)&&ValidBST(root->right,root,max);
    //代入有
    //root->left->val > root->val
    //root->right->val < root->val
    //这些都是要return false的
}

bool isValidBST(struct TreeNode* root) {
    return ValidBST(root,LONG_MIN,LONG_MAX);
}
*/


//2、改进如下(实在是太巧妙啦)
bool ValidBST(struct TreeNode* root,struct TreeNode* min,struct TreeNode* max) {
    if (root==NULL) return true;
    if (max!=NULL&&root->val >= max->val || min!=NULL&&root->val <= min->val) return false;

    return ValidBST(root->left,min,root)&&ValidBST(root->right,root,max);
}

bool isValidBST(struct TreeNode* root) {
    return ValidBST(root,NULL,NULL);
}



//方法二：线索二叉树
//参考答案：https://leetcode.com/problems/validate-binary-search-tree/discuss/32384/8ms-C-non-recursive-Morris-traversal

```