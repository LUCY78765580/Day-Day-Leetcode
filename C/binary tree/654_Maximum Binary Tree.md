## 654  Maximum Binary Tree
[题目地址](https://leetcode.com/problems/maximum-binary-tree/description/)
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

//最大二叉树

//方法：分治+递归
//这里与前面  108 Convert Sorted Array to Binary Search Tree 异曲同工

struct TreeNode* build(int* nums,int start,int end) {
    if (start>end) return NULL;

    //找到子树中最大值的位置 maxIndex
    //注意这里的i<=end(而不是i<end)
    int maxIndex=start;
    for (int i=start;i<=end;i++) {
        maxIndex=(nums[i]>nums[maxIndex])? i:maxIndex;
    }

    //创建root根节点并递归建树
    struct TreeNode* root=malloc(sizeof(struct TreeNode));
    root->val=nums[maxIndex];
    root->left=build(nums,start,maxIndex-1);
    root->right=build(nums,maxIndex+1,end);

    return root;
}

struct TreeNode* constructMaximumBinaryTree(int* nums, int numsSize) {
    if (nums==NULL) return NULL;
    return build(nums,0,numsSize-1);
}
```
