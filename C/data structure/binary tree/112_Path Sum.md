## 112  Path Sum
[题目地址](https://leetcode.com/problems/path-sum/description/)
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


//和为指定值的二叉树节点路径（判断是否存在）
//同样使用递归

//递归方法一
bool hasPathSum(struct TreeNode* root,int sum) {
    if (root==NULL) return false;
    if (root->left==NULL&&root->right==NULL&&root->val==sum) return true;

    return ( hasPathSum(root->left,sum - root->val) || hasPathSum(root->right,sum - root->val) );
}



//递归方法二
//思路：将查找路径（sum值）记录到数组res中，递归结束后对res进行遍历，检查是否存在res[i]==sum

//递归函数
void pathRecursive(struct TreeNode* root,int* res,int* count,int pathSum) {
    if (root==NULL) return ;
    //注意题干中  root-to-leaf path
    //所以一直查找到叶节点，才将pathSum记入res中

    pathSum+=root->val;
    if (root&&root->left==NULL&&root->right==NULL) {
        res[(*count)++]=pathSum;
        return ;
    }

    pathRecursive(root->left,res,count,pathSum);
    pathRecursive(root->right,res,count,pathSum);
}


//主函数
bool hasPathSum(struct TreeNode* root, int sum) {
    if (root==NULL) return false;

    int* res=(int*)malloc(10000*sizeof(int));
    int size=0;
    int flag=0;

    pathRecursive(root,res,&size,0);

    for (int i=0;i<size;i++) {
        if (res[i]==sum) {
            flag=1;
            break;
        }
    }
    return flag;
}
```