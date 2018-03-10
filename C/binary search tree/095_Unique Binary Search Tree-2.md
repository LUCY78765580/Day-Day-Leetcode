## 095 Unique Binary Search Trees II
[题目地址](https://leetcode.com/problems/unique-binary-search-trees-ii/description/)
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

//可行的二叉查找树(返回具体BST)
//与前面 096  Unique Binary Search Trees 类似,本质上也是动态规划的题

/*
1. 每一次都在一个范围内随机选取一个结点作为根。
2. 每选取一个结点作为根，就把树切分成左右两个子树，直至该结点左右子树为空。

大致思路如上，具体来说，采取的是自底向上的求解过程。
1. 选出根结点后应该先分别求解该根的左右子树集合，也就是根的左子树有若干种，它们组成左子树集合，根的右子树有若干种，它们组成右子树集合。
2. 然后将左右子树相互配对，每一个左子树都与所有右子树匹配，每一个右子树都与所有的左子树匹配。然后将两个子树插在根结点上。
3. 最后，把根结点放入res中
*/

//辅助函数
struct TreeNode* Help(struct TreeNode*** res,int* returnSize,int x) {
    struct TreeNode* temp=NULL;
    if (x>-1) {
        temp=(struct TreeNode*)malloc(sizeof(struct TreeNode));
        temp->val=x;
        temp->left=temp->right=NULL;
    }

    *returnSize +=1;
    (*res)=(int**)realloc(*res,sizeof(struct TreeNode**)*(*returnSize));
    (*res)[*returnSize-1]=temp;
    return temp;
}

//递归函数
struct TreeNode** generate(int start,int end,int* returnSize) {
    struct TreeNode** res=(struct TreeNode**)malloc(sizeof(struct TreeNode*));
    if (start>end) {
        Help(&res,returnSize,-1);
        return res;
    }

    //下面这个可以忽略
    /*if (start==end) {
        Help(&res,returnSize,start);
        return res;
    }
    */

    for (int i=start;i<=end;i++) {
        int sizeLeft=0,sizeRight=0;
        struct TreeNode** resLeft=generate(start,i-1,&sizeLeft);
        struct TreeNode** resRight=generate(i+1,end,&sizeRight);

        for (int j=0;j<sizeLeft;j++) {
            for (int k=0;k<sizeRight;k++) {
                struct TreeNode* root=Help(&res,returnSize,i);
                root->left=resLeft[j];
                root->right=resRight[k];
            }
        }
    }

    return res;
}

//总函数
struct TreeNode** generateTrees(int n, int* returnSize) {
    if (n==NULL) return NULL;

    *returnSize=0;
    return generate(1,n,returnSize);
}

//参考答案1：https://leetcode.com/problems/unique-binary-search-trees-ii/discuss/31494 (递归JAVA版本)
//参考答案2：https://leetcode.com/problems/unique-binary-search-trees-ii/discuss/31508 (递归JAVA版本)
//参考答案3：https://leetcode.com/problems/unique-binary-search-trees-ii/discuss/31557 (C语言版本)
```