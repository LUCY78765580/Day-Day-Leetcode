## 144  Binary Tree Preorder Traversal
[题目地址](https://leetcode.com/problems/binary-tree-preorder-traversal/description/)
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

//先序遍历，与中序遍历类似

//方法一：迭代+堆栈
//只需将操作放在Push前面
int* preorderTraversal(struct TreeNode* root, int* returnSize) {
    int total=64;
    int* res=(int*)malloc(total*sizeof(int));
    struct TreeNode** stack=(struct TreeNode**)malloc(total*sizeof(struct TreeNode*));

    int size=0;
    int len=0;

    while (len>0 || root!=NULL) {
        if (root!=NULL) {
            res[size++]=root->val;
            stack[len++]=root;
            root=root->left;
        }
        else {
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



//方法二：递归
void preTraversal(struct TreeNode* root,int* res,int* count) {
    if (root) {
        res[(*count)++]=root->val;
        preTraversal(root->left,res,count);
        preTraversal(root->right,res,count);
    }
}

int* preorderTraversal(struct TreeNode* root, int* returnSize) {
    //边界条件
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


//方法三：morris traversal（线索二叉树）
int* preorderTraversal(struct TreeNode* root, int* returnSize) {
    int total=64;
    int* res=(int*)malloc(total*sizeof(int));
    int size=0;

    struct TreeNode* p=root;
    struct TreeNode* temp=NULL;

    while (p) {
        if (p->left!=NULL) {
            temp=p->left;
            while (temp->right!=NULL&&temp->right!=p)
                temp=temp->right;
            //类似点F的情况
            if (temp->right==NULL) {
                res[size++]=p->val;
                temp->right=p;
                p=p->left;
            }
            //类似点B的情况
            else {
                temp->right=NULL;
                p=p->right;
            }
        }
        else {
            //类似点A的情况
            res[size++]=p->val;
            p=p->right;
        }

        if (size==total) {
            total<<=1;
            res=(int*)realloc(res,total*sizeof(int));
        }
    }

    *returnSize=size;
    return res;
}
```