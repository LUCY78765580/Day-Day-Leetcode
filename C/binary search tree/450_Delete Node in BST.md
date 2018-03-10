## 450  Delete Node in a BST
[题目地址](https://leetcode.com/problems/delete-node-in-a-bst/description/)
<br>
<br>
<br>
<br>

![](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/BST001.jpg)

<br>
<br>

![](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/BST002.jpg)

<br>
<br>

![](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/BST003.jpg)


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

//删除BST中节点
//同样适用递归

//由上图，分为三种情况
/*
1、删除叶节点
2、删除节点，其只有一个孩子节点
3、待删除节点有左右两个子树
*/

//最小元素一定在最左分支的端点上
struct TreeNode* findMin(struct TreeNode* root) {
    while (root->left)
        root=root->left;

    return root;
}

struct TreeNode* deleteNode(struct TreeNode* root, int key) {
    if (root==NULL) return NULL;
    else if (key < root->val) {
        root->left=deleteNode(root->left,key);
    }
    else if (key > root->val) {
        root->right=deleteNode(root->right,key);
    }
    //root->val==key时
    else {
        //这里采用右子树的最小值，填充删除节点temp
        //将temp的值赋值给当前位置root
        //最后递归删除掉temp节点,删掉的是temp值
        if (root->left&&root->right) {
            struct TreeNode* temp=findMin(root->right);
            root->val=temp->val;
            root->right=deleteNode(root->right,temp->val);
            //注意这里的deleteNode(root->right,temp->val)
            //不要写成key了
        }
        else {
            //有左孩子、或者无子节点
            //有右孩子、或者无子节点
            //struct TreeNode* temp=root;
            if (!root->right)
                root=root->left;
            else if (!root->left)
                root=root->right;

            //free(temp);
        }
    }

    return root;
}
```