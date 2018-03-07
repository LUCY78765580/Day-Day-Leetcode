## 226  Invert Binary Tree
[题目地址](https://leetcode.com/problems/invert-binary-tree/description/)
<br>
<br>
<br>

![](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/binary-tree008.jpg)
<br>
##### 前一阵homebrew作者面试谷歌被拒，原因之一是这位老兄无法反转出二叉树 :flushed:
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


//翻转二叉树
//使用递归


//dfs递归形式一
struct TreeNode* invertTree(struct TreeNode* root) {
    if (root==NULL) return NULL;

    struct TreeNode* temp;
    temp=root->left;
    root->left=root->right;
    root->right=temp;

    root->left=invertTree(root->left);
    root->right=invertTree(root->right);
    return root;
}



//dfs递归形式二：更简洁的版本
//简直了666
struct TreeNode* invertTree(struct TreeNode* root) {
    if (root==NULL) return NULL;

    struct TreeNode* temp=root->left;
    root->left=invertTree(root->right);
    root->right=invertTree(temp);

    return root;
}


//方法三：比特位操作（某位网友的想法）
//代码通过（看不懂啊啊）
#define swap_nodes(a,b) \
({                      \
    a = (((struct TreeNode *) ((uintptr_t) (a) ^ (uintptr_t) (b)))); \
    b = (((struct TreeNode *) ((uintptr_t) (a) ^ (uintptr_t) (b)))); \
    a = (((struct TreeNode *) ((uintptr_t) (a) ^ (uintptr_t) (b)))); \
})

struct TreeNode* invertTree(struct TreeNode* root) {
    if (root==NULL) return NULL;

    invertTree(root->left);
    invertTree(root->right);
    swap_nodes(root->left, root->right);

    return root;
}
```
