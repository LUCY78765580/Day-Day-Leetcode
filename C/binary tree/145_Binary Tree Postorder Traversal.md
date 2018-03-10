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

//二叉树的后序遍历
//遍历顺序：左->右->根
//递归方式+非递归方式（堆栈/线索二叉树）
//方法与 114 Binary Tree Preorder Traversal 类似


//方法一：递归方式
void postTraversal(struct TreeNode* root,int* res,int* count) {
    if (root==NULL) return ;

    //与先序遍历不同之处
    postTraversal(root->left,res,count);
    postTraversal(root->right,res,count);
    res[(*count)++]=root->val;
}

int* postorderTraversal(struct TreeNode* root,int* returnSize) {
    if (root==NULL) {
        *returnSize=0;
        return NULL;
    }

    int* res=(int*)malloc(1000*sizeof(int));
    int size=0;

    postTraversal(root,res,&size);
    *returnSize=size;
    return res;
}



/*
//方法二：非递归+堆栈
//这里就比较难了,参考博客：http://blog.csdn.net/zhangxiangDavaid/article/details/37115355

/*后序遍历的难点在于：
需要判断上次访问的节点是位于左子树，还是右子树。

1、若是位于左子树，则需跳过根节点，先进入右子树，再回头访问根节点；

2、若是位于右子树，则直接访问根节点
*/


//添加一个lastVisit节点，标记上一个访问对象
int* postorderTraversal(struct TreeNode* root, int* returnSize) {
    int total=64;
    int* res=(int*)malloc(total*sizeof(int));
    struct TreeNode** stack=(struct TreeNode**)malloc(total*sizeof(struct TreeNode*));
    struct TreeNode* lastVisit=NULL;

    int size=0;
    int len=0;

    //先将p指针，移动到左子树最下面
    //根节点第一次进栈
    while (root!=NULL) {
        stack[len++]=root;
        root=root->left;
    }

    while (len>0) {
        //这时已经到达左子树最底端，root已经为NULL
        //弹出一个节点
        root=stack[--len];

        //上面左子树刚刚被访问过，这时需进入右子树
        //根节点第二次进入栈
        //接着进入右子树(右子树必不为空)
        if (root->right!=NULL&&root->right!=lastVisit) {
            stack[len++]=root;
            root=root->right;

            //对右子树又采用与开始一样的方式
            while (root) {
                stack[len++]=root;
                root=root->left;
            }
        }
        //根节点被访问的前提是：1、无右子树 2、右子树已经被访问过
        else {
            res[size++]=root->val;
            lastVisit=root;
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
//参考博客：http://www.cnblogs.com/AnnieKim/archive/2013/06/15/MorrisTraversal.html
//有bug，不能得正确答案。。。

void reverse(struct TreeNode* from,struct TreeNode* to) {
    if (from == to) return;
    struct TreeNode *x = from, *y = from->right, *z;

    while (x!=to) {
        z = y->right;
        y->right = x;
        x = y;
        y = z;
    }
}

void addReverse(struct TreeNode* from,struct TreeNode* to,int* res,int size) {
    reverse(from, to);
    struct TreeNode* tmp = to;

    while (tmp!=from) {
        res[size++]=tmp->val;
        tmp = tmp->right;
    }

    reverse(to, from);
}


int* postorderTraversal(struct TreeNode* root, int* returnSize) {
    int total=64;
    int* res=(int*)malloc(total*sizeof(int));
    int size=0;

    struct TreeNode* dump=(struct TreeNode*)malloc(sizeof(struct TreeNode));
    dump->val=0;
    dump->left=root;
    dump->right=NULL;

    struct TreeNode* p=dump;
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
            //temp->right不为NULL，此时已经成环，并且temp处于根节点p位置
            //这里与中序遍历有些像，但是要倒序输出从当前节点的左孩子(p->left)到该前驱节点(p)这条路径上的所有节点
            //就是这个圈上的节点，调用addReverse函数
            else {
                addReverse(p->left,temp,res,size);
                temp->right=NULL;
                p=p->right;
            }
        }
        //2) p->left为NULL时
        else {
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