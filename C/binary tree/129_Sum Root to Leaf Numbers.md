## 129  Sum Root to Leaf Numbers
[题目地址](https://leetcode.com/problems/sum-root-to-leaf-numbers/description/)
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


//根到叶节点数字之和
//使用递归

/*
遇到的问题：
1. 如何分情况？
2. 如何递归到该点的时候该如何操作？
3. 由跟节点到叶子节点的时候，每进入一层就需要把前面的值*10，如何处理这个值？总值又是如何保存的呢？

问题处理：
1)分情况就是：一、空节点 二、叶子节点  三、一般节点（包括只有左孩子，只有右孩子，两个孩子都有）
2)递归到该点的时候就把这点的值与前面的值乘以10，然后相加
3)需要如何保存前面的值？以参数传递的方式保存，以供下一层使用。总值可以以两种方式保存：参数传递方式，返回函数值方式。
*/

/*
递归主要考虑递归条件和结束条件。这道题目标是把从根到叶子节点的所有路径得到的整数都累加起来，
1、递归条件即是把当前的sum乘以10并且加上当前节点传入下一函数，进行递归，最终把左右子树的总和相加。
2、结束条件的话就是如果一个节点是叶子，那么我们应该累加到结果总和中，如果节点到了空节点，则不是叶子节点，不需要加入到结果中，直接返回0即可。

算法的本质是一次先序遍历，所以时间是O(n)，空间是栈大小，O(logn)
*/

//参考博客1：http://blog.csdn.net/linhuanmars/article/details/22913699
//参考博客2：http://blog.csdn.net/kenden23/article/details/14100851
//这两个博客都分析得很到位

//递归函数单独拎出来
int sumRecursive(struct TreeNode* root,int sum) {
    //结束条件
    if (root==NULL) return 0;
    if (root->left==NULL&&root->right==NULL)
        return sum*10+root->val;

    //递归条件
    return sumRecursive(root->left,sum*10+root->val)+sumRecursive(root->right,sum*10+root->val);
}

int sumNumbers(struct TreeNode* root) {
    return sumRecursive(root,0);
}
```