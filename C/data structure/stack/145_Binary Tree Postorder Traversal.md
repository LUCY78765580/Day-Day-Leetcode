## 145  Binary Tree Postorder Traversal
[题目地址](https://leetcode.com/problems/binary-tree-postorder-traversal/description/)
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
/**
 * Return an array of size *returnSize.
 * Note: The returned array must be malloced, assume caller calls free().
 */

//后序遍历


//方法一：迭代＋堆栈
//这里用堆栈实现，难，暂且搁置


//方法二：递归
//递归方法就是快呀
void postTraversal(struct TreeNode* root,int* res,int* count) {
    if (root) {
        postTraversal(root->left,res,count);
        postTraversal(root->right,res,count);
        res[(*count)++]=root->val;
    }
}

int* postorderTraversal(struct TreeNode* root, int* returnSize) {
    int* res=(int*)malloc(1000*sizeof(int));
    int size=0;

    postTraversal(root,res,&size);
    *returnSize=size;
    return res;
}
```