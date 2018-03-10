## 105  Construct Binary Tree from Preorder and Inorder Traversal
[题目地址](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/description/)
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

//由先序遍历、中序遍历结果构造二叉树

//方法一：分治+递归

/*
总体思路：
1、首先由先序遍历，得到根节点值root=*preorder，并且构造树根节点
2、对中序遍历数组，进行遍历，找到root的位置i
3、以i为分界线，对左右子树，分别进行同样的递归
   这里的关键就是，重新分配时左右子树位置以及大小确定
*/

struct TreeNode* buildTree(int* preorder,int preorderSize,int* inorder,int inorderSize) {
    if (preorderSize==0 || inorderSize==0) return NULL;

    struct TreeNode* root=(struct TreeNode*)malloc(sizeof(struct TreeNode));
    root->val=*preorder;

    int i=0;
    while (inorder[i]!=root->val) i++;


    root->left=buildTree(preorder+1,i,inorder,i);
    root->right=buildTree(preorder+1+i;preorderSize-1-i;inorder+1+i;inorderSize-1-i);
    return root;
}


struct TreeNode* buildTree(int* preorder, int preorderSize, int* inorder, int inorderSize) {
    if (preorderSize==0 || inorderSize==0) return NULL;

    //找到root并构建根节点（不要忘记malloc）
    struct TreeNode* root=(struct TreeNode*)malloc(sizeof(struct TreeNode));
    root->val=*preorder;

    int i=0;
    while (inorder[i]!=root->val) i++;
    //实际上是第i+1个数为root,root的左边有i个数
    //preorder+i+1 和 inorder+i+1,是因为前面都有左子树（i个）和根节点（1个）

    root->left=buildTree(preorder+1,i,inorder,i);
    root->right=buildTree(preorder+i+1,preorderSize-i-1,inorder+i+1,inorderSize-i-1);
    return root;
}


//方法二：迭代(使用堆栈)

//方法三：hash_map
//参考答案：https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/discuss/34691
//又是一个看不懂的
```