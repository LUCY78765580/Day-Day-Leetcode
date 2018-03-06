## 100  Same Tree
[题目地址](https://leetcode.com/problems/same-tree/description/)
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


//判断是否同一棵二叉树
//使用递归


//1、列举法
bool isSameTree(struct TreeNode* p,struct TreeNode* q) {
    //如果两树都不存在
    if (!p&&!q) return true;
    //如果有一棵树不存在
    if (!p || !q) return false;

    if (p->val==q->val) {
        return isSameTree(p->left,q->left)&&isSameTree(p->right,q->right);
    }
    else return false;
}


//2、简写形式
bool isSameTree(struct TreeNode* p,struct TreeNode* q) {
    if (!p&&!q) return true;
    return p&&q&&p->val==q->val&&isSameTree(p->left,q->left)&&isSameTree(p->right,q->right);
}
```