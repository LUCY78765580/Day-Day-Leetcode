## 144  Binary Tree Preorder Traversal
[题目地址](https://leetcode.com/problems/binary-tree-preorder-traversal/description/)
<br>
<br>
<br>

![线索二叉树-先序遍历](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/binary-tree009.jpg)
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

//二叉树的先序遍历
//遍历顺序：根->左->右
//递归方式+非递归方式（堆栈/线索二叉树）


//方法一：递归方式
void preTraversal(struct TreeNode* root,int* res,int* count) {
    if (root==NULL) return ;

    res[(*count)++]=root->val;
    preTraversal(root->left,res,count);
    preTraversal(root->right,res,count);
}

int* preorderTraversal(struct TreeNode* root,int* returnSize) {
    if (root==NULL) {
        *returnSize=0;
        return NULL;
    }

    int* res=(int*)malloc(1000*sizeof(int));
    int size=0;

    preTraversal(root,res,&size);
    *returnSize=size;
    return res;
}



//方法二：非递归+堆栈
//total为res数组的初始长度,也是stack初始长度
//size和len分别为res,stack 实际长度

int* preorderTraversal(struct TreeNode* root, int* returnSize) {
    int total=64;
    int* res=(int*)malloc(total*sizeof(int));
    struct TreeNode** stack=(struct TreeNode**)malloc(total*sizeof(struct TreeNode*));

    int size=0;
    int len=0;

    while (len>0 || root!=NULL) {
        //len>0说明stack不为空,如果节点root不为NULL
        //值存入res,将节点push入stack
        //root往左边走
        if (root!=NULL) {
            res[size++]=root->val;
            stack[len++]=root;
            root=root->left;
        }
        else {
            //后面要检查内存，这里的else加上是有必要的
            //如果节点为NULL(叶节点)
            //将节点pop出stack
            //root往右边走
            root=stack[--len];
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


//方法三：非递归+morris traversal（线索二叉树）
//空间复杂度仅仅为O(1),即常量空间
//参考博客：http://www.cnblogs.com/AnnieKim/archive/2013/06/15/MorrisTraversal.html
int* preorderTraversal(struct TreeNode* root, int* returnSize) {
    int total=64;
    int* res=(int*)malloc(total*sizeof(int));
    int size=0;

    struct TreeNode* p=root;
    struct TreeNode* temp;

    while (p) {
        //1) 左子节点不为NULL
        //找到左子树最右边 的右子节点temp，分temp->right为NULL和不为NULL两种讨论
        if (p->left!=NULL) {
            temp=p->left;
            while (temp->right!=NULL&&temp->right!=p)
                temp=temp->right;

            //如果temp->right==NULL
            //记录p->val的值，连接temp和p，成为一个环
            //p往左边走
            if (temp->right==NULL) {
                res[size++]=p->val;
                temp->right=p;
                p=p->left;
            }
            //如果temp->right!=NULL，说明已经成环（并且此时temp在根节点p的位置）
            //断开这个环
            //p往右边走
            else {
                temp->right=NULL;
                p=p->right;
            }
        }

        //2) 左子节点为NULL
        //将当前节点存入res
        //p往右边走
        else {
            res[size++]=p->val;
            p=p->right;
        }

        //检查内存并重新分配
        if (size==total) {
            total<<=1;
            res=(int*)realloc(res,total*sizeof(int));
        }
    }

    *returnSize=size;
    return res;
}
```