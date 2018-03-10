## 106  Construct Binary Tree from Inorder and Postorder Traversal
[题目地址](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/description/)
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

//由中序遍历、后序遍历结果构造二叉树
//方法和前面105题一样，参数注意一下即可,root为postorder最后一个值


struct TreeNode* buildTree(int* inorder, int inorderSize, int* postorder, int postorderSize) {
    if (inorderSize==0||postorderSize==0) return NULL;

    struct TreeNode* root=(struct TreeNode*)malloc(sizeof(struct TreeNode));
    root->val=postorder[postorderSize-1];

    int i=0;
    while (inorder[i]!=root->val) i++;
    //实际上是第i+1个数为root,root的左边有i个数
    //inorder+i加1，是因为root在中间，左边有i个数
    //postorder+i，是因为root已经在后面了

    root->left=buildTree(inorder,i,postorder,i);
    root->right=buildTree(inorder+i+1,inorderSize-i-1,postorder+i,postorderSize-i-1);
    return root;
}
```