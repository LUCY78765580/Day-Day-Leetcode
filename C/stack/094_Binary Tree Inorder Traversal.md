## 094  Binary Tree Inorder Traversal
[题目地址](https://leetcode.com/problems/binary-tree-inorder-traversal/description/)
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


//审题：数组必需要自己malloc，返回长度是*returnSize

//方法一：堆栈+迭代
//堆栈采用数组形式
int* inorderTraversal(struct TreeNode* root, int* returnSize) {
    int total=64;
    int* res=(int*)malloc(total*sizeof(int));
    struct TreeNode** stack=(struct TreeNode**)malloc(total*sizeof(struct TreeNode*));

    int size=0;
    int len=0;

    while (len>0 || root!=NULL) {
        if (root!=NULL) {
            //压栈push
            stack[len++]=root;
            root=root->left;
        }
        else {
            //弹出pop
            root=stack[--len];
            res[size++]=root->val;
            root=root->right;
        }

        //检查内存并重新分配
        if (size==total||len==total) {
            total<<=1;
            res=(int*)realloc(res,total*sizeof(int));
            stack=(struct TreeNode**)realloc(stack,total*sizeof(struct TreeNode*));
        }
    }

    *returnSize=size;
    return res;
}




//方法二：递归
void inTraversal(struct TreeNode* root,int* res,int* size) {
    if (root) {
        inTraversal(root->left,res,size);
        res[(*size)++]=root->val;
        inTraversal(root->right,res,size);
    }
}

int* inorderTraversal(struct TreeNode* root, int* returnSize) {
    //注意边界条件
    if (root==NULL) {
        *returnSize=0;
        return NULL;
    }
    int count=0;
    int* res=(int*)malloc(1000*sizeof(int));

    //注意这里的&count，以及上面函数中的*count
    inTraversal(root,res,&count);
    *returnSize=count;
    return res;
}




//方法三：morris traversal（线索二叉树）
//参考博客1：http://blog.csdn.net/u011293309/article/details/9235653
//参考博客2：http://www.cnblogs.com/AnnieKim/archive/2013/06/15/MorrisTraversal.html

int* inorderTraversal(struct TreeNode* root, int* returnSize) {
    int total=64;
    int* res=(int*)malloc(total*sizeof(int));
    int size=0;
    struct TreeNode* p=root;
    struct TreeNode* temp=NULL;

    while (p) {
        //p->left不为NULL的时候，例如F点
        if (p->left!=NULL) {
            temp=p->left;
            while (temp->right!=NULL&&temp->right!=p)
                temp=temp->right;

            //temp->right==NULL的时候
            if (temp->right==NULL) {
                temp->right=p;
                p=p->left;
            }
            //temp->right==p的时候，例如B点
            else {
                res[size++]=p->val;
                temp->right=NULL;
                p=p->right;
            }
        }
        //p->left为NULL的时候，例如A点
        else {
            res[size++]=p->val;
            p=p->right;
        }

        //检查内存是否足够并重新分配
        if (size==total) {
            total<<=1;
            res=(int*)realloc(res,total*sizeof(int));
        }
    }

    *returnSize=size;
    return res;
}
```