## 108  Convert Sorted Array to Binary Search Tree
[题目地址](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/description/)
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

//排序数组转变为二叉搜索树BST
//BST特性：1、左节点及以下节点的值比它小；2、右节点及以下节点的值比它大。
//这里因为已经排好序了，所以直接用递归+二分法
//如果遇到没有排序的题，要先排序

//使用递归+二分法，简单方便哈哈
struct TreeNode* helper(int* nums,int start,int end) {
    if (start>end) return NULL;

    struct TreeNode* root=(struct TreeNode*)malloc(sizeof(struct TreeNode));
    int mid=(start+end)/2;

    root->val=nums[mid];
    root->left=helper(nums,start,mid-1);
    root->right=helper(nums,mid+1,end);

    return root;
}

struct TreeNode* sortedArrayToBST(int* nums,int numsSize) {
    return helper(nums,0,numsSize-1);
}
```