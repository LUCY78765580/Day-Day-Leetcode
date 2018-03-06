## 124  Binary Tree Maximum Path Sum
[题目地址](https://leetcode.com/problems/binary-tree-maximum-path-sum/description/)
<br>
<br>
<br>


![](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/binary-tree007.jpg)

如上图所示，需要考虑以上两种情况：

**1、 左子树或者右子树中存有最大路径，不能和根节点形成一个路径**

**2、左子树、右子树、根节点形成最大路径**


动态规划，对每一个节点，以该节点为根节点\左子树\右子树路径最大值分别为value、lmax、rmax，
(!!! 注意：这里的lmax和rmax指的是从左/右子节点出发的某一条单向路径)

那么有：**value = value + (lmax>0?lmax:0) + (rmax>0?rmax:0) ;**

由上，递归计算通过每个节点的**最大路径和 (lmax + rmax + val)**，
与通过该节点的**最大子路径的分支(max(lmax,rmax) + val)**, 每次更新一下最大路径和就可以了
<br>
<br>

```c
//二叉树的最大路径和
//还是要用到递归
//涉及动态规划，一脸蒙圈


//特别注意maxValue定义（全局变量）
//int maxValue=INT_MIN;在前面是错误的，只能是单独的 int maxValue
int maxValue;
int max(int a,int b) {
    return a>b?a:b;
}

int maxPath(struct TreeNode* root) {
    if (root==NULL) return 0;

    int left=max(0,maxPath(root->left));
    int right=max(0,maxPath(root->right));
    maxValue=max(maxValue,left+right+root->val);

    return root->val+max(left,right);
}

int maxPathSum(struct TreeNode* root) {
    maxValue=INT_MIN;
    maxPath(root);
    return maxValue;
}
```