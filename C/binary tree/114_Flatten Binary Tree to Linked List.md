## 114  Flatten Binary Tree to Linked List
[题目地址](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/description/)
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

//二叉树转为有序双链表
//题目分析：将给定的二叉树转化为“只有右孩子节点”的链表（树）

/*
1、先判断左子树是否为空，不为空就寻找到树根的左孩子节点，
2、然后寻找该节点是否有右孩子，如果有继续寻找，直到找到属于叶子节点的右孩子。
3、此时，该节点的右子树“指向”当前树的右子树，并将当前左子树变为树根的右孩子，将整棵树左孩子置为空。
4、最后，根节点“指向”根节点的右孩子，继续上述操作，直到整棵树遍历完即得到结果
*/


//方法一：使用递归+后序遍历
//难理解的emmm

//递归函数单独拎出来
struct TreeNode* flat(struct TreeNode* root,struct TreeNode* pre) {
    if (root==NULL) return pre;

    //右边就是结果pre,左边全是NULL
    root->right = flat(root->left, flat(root->right, pre));
    root->left=NULL;

    //pre=flat(root->right,pre);
    //pre=flat(root->left,pre);
    //root->right=pre
    //注意这里的right、left位置不可以放反的,第二句中的pre即为上一句的结果

    pre=root;
    return pre;
}

void flatten(struct TreeNode* root) {
    flat(root,NULL);
}



//方法二：借鉴Morris traversal线索二叉树
//参考博客1：http://www.cnblogs.com/AnnieKim/archive/2013/06/15/MorrisTraversal.html
//参考博客2：http://blog.csdn.net/u011293309/article/details/9235653

void flatten(struct TreeNode* root) {
    struct TreeNode* p=root;
    struct TreeNode* temp;

    while (p) {
        //如果左子节点p->left不为空，temp=p->left,并且遍历找到叶子节点的右孩子
        //将右子节点p->right，接到上述找到的叶子节点的右孩子后面

        //后来的右子节点p->right,赋值为p->left，即将左边一大块接过来
        //后来的左子节点p->left，赋值为NULL
        if (p->left!=NULL) {
            temp=p->left;
            while (temp->right!=NULL)
                temp=temp->right;

            temp->right=p->right;
            p->right=p->left;
            p->left=NULL;
        }
        //如果左子节点为空，p往下走，p=p->right
        //相当于只需要按照上述同样方式，再调节p->right这一课子树就可以了
        else {
            p=p->right;
        }
    }
}
```