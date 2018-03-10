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


//二叉树的中序遍历
//遍历顺序：左->根->右
//递归方式+非递归方式（堆栈/线索二叉树）
//方法与 114 Binary Tree Preorder Traversal 类似


//方法一：递归方式
void inTraversal(struct TreeNode* root,int* res,int* count) {
    if (root==NULL) return ;

    //与先序遍历不同之处
    inTraversal(root->left,res,count);
    res[(*count)++]=root->val;
    inTraversal(root->right,res,count);
}

int* inorderTraversal(struct TreeNode* root,int* returnSize) {
    if (root==NULL) {
        *returnSize=0;
        return NULL;
    }

    int* res=(int*)malloc(1000*sizeof(int));
    int size=0;

    inTraversal(root,res,&size);
    *returnSize=size;
    return res;
}



//方法二：非递归+堆栈
int* inorderTraversal(struct TreeNode* root, int* returnSize) {
    int total=64;
    int* res=(int*)malloc(total*sizeof(int));
    struct TreeNode** stack=(struct TreeNode**)malloc(total*sizeof(struct TreeNode*));

    int size=0;
    int len=0;

    while (len>0 || root!=NULL) {
        if (root!=NULL) {
            stack[len++]=root;
            root=root->left;
        }
        else {
            //与先序遍历不同之处
            //一路往左边走并压栈push，一直到最左边；接着节点pop弹出，存入res;再往右边走
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



//方法三：非递归+morris traversal（线索二叉树）
int* inorderTraversal(struct TreeNode* root, int* returnSize) {
    int total=64;
    int* res=(int*)malloc(total*sizeof(int));
    int size=0;

    struct TreeNode* p=root;
    struct TreeNode* temp;

    while (p) {
        //1) p->left不为NULL时
        if (p->left!=NULL) {
            temp=p->left;
            while (temp->right!=NULL&&temp->right!=p)
                temp=temp->right;

            if (temp->right==NULL) {
                temp->right=p;
                p=p->left;
            }
            //p->right不为NULL时，说明已经成环
            //temp此时回到p的位置,这时才开始将p->val存入res
            //这是与先序遍历唯一不同之处
            else {
                res[size++]=p->val;
                temp->right=NULL;
                p=p->right;
            }
        }
        //2) p->left为NULL时
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