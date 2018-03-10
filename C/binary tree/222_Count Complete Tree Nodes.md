## 222  Count Complete Tree Nodes
[题目地址](https://leetcode.com/problems/count-complete-tree-nodes/description/)
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

//求二叉树的节点个数
//注意：题目限定了该二叉树是完全二叉树


//方法一：不讲道理的递归
/*
暴力解决，使用递归依次遍历所有节点并统计
思路使用于所有二叉树，这里超时了
*/
int countNodes(struct TreeNode* root) {
    if (root==NULL) return 0;
    return 1+countNodes(root->left)+countNodes(root->right);
}



//方法二：根据完全二叉树性质求解
/*
完全二叉树：若二叉树深度为h，除第 h 层外，其它各层 (1～h-1) 的结点数都达到最大个数，第 h 层所有的结点都连续集中在最左边，

1、对于一个节点node，计算它最左端的节点到node的深度为leftDepth，计算它最右端的节点到node的深度是rightDepth；
2、如果leftDepth和rightDepth相等，那么以node为根节点的树是一棵满二叉树，此时以node为根节点的树的节点个数是pow(2,leftDepth)-1；
3、如果leftDepth和rightDepth不相等，递归求解node的左子树的节点数和右子树的节点数。注意此时左子树必定是满二叉树了。
*/

int countNodes(struct TreeNode* root) {
    if (root==NULL) return 0;

    int leftDepth=0;int rightDepth=0;
    for (struct TreeNode* node=root;node!=NULL;node=node->left)
        leftDepth++;
    for (struct TreeNode* node=root;node!=NULL;node=node->right)
        rightDepth++;

    if (leftDepth==rightDepth) {
        return (1<<leftDepth)-1;
    }
    return countNodes(root->left)+countNodes(root->right)+1;
}
```