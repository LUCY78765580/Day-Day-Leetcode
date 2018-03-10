## 235  Lowest Common Ancestor of a Binary Search Tree
[题目地址](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/description/)
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

//BST最近公共祖先
//"LCA"问题
//与236 Lowest Common Ancestor of a Binary Tree 非常相似

/*
因为二叉搜索树已经排序好了,很简单的思路就是看两个值在root的哪边：
1、两个值都在左边，则LCA在左边
2、两个值都在右边，则LCA在右边
3、一个在左一个在右，则说明LCA就是当前的root节点。
*/
/*
总：if (p->val > q->val) return lowestCommonAncestor(root,q,p);
1、if (root==NULL || root->val >= p->val || root->val <= q->val) return root;
2、if (root->val > q->val) return lowestCommonAncestor(root->left,p,q);
3、if (root->val < p->val) return lowestCommonAncestor(root->right,p,q);
*/

struct TreeNode* lowestCommonAncestor(struct TreeNode* root, struct TreeNode* p, struct TreeNode* q) {
    if (p->val > q->val) return lowestCommonAncestor(root,q,p);
    if (root->val > q->val)
        return lowestCommonAncestor(root->left,p,q);
    else if (root->val < p->val)
        return lowestCommonAncestor(root->right,p,q);
    else
        return root;
    //if (root==NULL || root->val >= p->val || root->val <= q->val) return root;
}
```