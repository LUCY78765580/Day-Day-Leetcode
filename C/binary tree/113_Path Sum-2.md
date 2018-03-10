## 112  Path Sum II
[题目地址](https://leetcode.com/problems/path-sum-ii/description/)
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
 * Return an array of arrays of size *returnSize.
 * The sizes of the arrays are returned as *columnSizes array.
 * Note: Both returned array and *columnSizes array must be malloced, assume caller calls free().
 */

//和为指定值的二叉树节点路径（求具体路径）
//同样用递归
//增加了将路径存入数组这些操作,因为要记录具体路径，所有要判断树的深度


/*memcpy函数：void *memcpy(void *str1, const void *str2, size_t n) ，从存储区 str2复制 n个字符到存储区 str1，
1、与strcpy相比，memcpy遇到’\0’不结束，而且一定会复制完n个字节
所以一般格式：memcpy(dest, src, strlen(src)+1); 注意这里的strlen
2、memcpy只能用于char类型
参考博客：http://blog.csdn.net/xiaominkong123/article/details/51733528
        https://zhidao.baidu.com/question/1606289554770956107.html
*/


//求树的最大深度
int maxDepth(struct TreeNode* root) {
    if (!root) return 0;
    else {
        int L=maxDepth(root->left);
        int R=maxDepth(root->right);
        int M=(L>R)?L:R;

        return M+1;
    }
}

//递归函数
void pathRecursive(struct TreeNode* root,int** res,int* solution,int sum,int len,int** column_sizes,int* count) {
    //注意 root-to-leaf paths,所以需要加上root->left==NULL和root->right==NULL
    //memcpy(dest, src, strlen(src)+1); 注意这里的solution[len++]

    if (root==NULL) ;
    solution[(*len)++]=root->val;
    if (root->val==sum&&root->left==NULL&&root->right==NULL) {
        res[*count]=(int*)malloc(len*sizeof(int));
        memcpy(res[*count],solution,len*sizeof(int));
        (*column_sizes)[*count]=len;
        (*count)++;
        return ;

        //如果有else的话，这里的return可省略
        //否则报错（指针变成野指针？）
    }

    pathRecursive(root->left,res,solution,sum-root->val,len,column_sizes,count);
    pathRecursive(root->right,res,solution,sum-root->val,len,column_sizes,count);
}

//总函数
//res数组记录具体路径，*returnSize 代表数组res的长度
//columnSizes记录res中，每个小数组的长度
//depth 代表树最大深度
//solution 代表递归过程中临时数组，存放具体路径;depth即为solution长度

int** pathSum(struct TreeNode* root,int sum,int** columnSizes,int* returnSize) {
    if (root==NULL) return NULL;
    *returnSize=0;

    int depth=maxDepth(root);
    int** res=(int**)malloc(1000*sizeof(int*));
    *columnSizes=(int*)malloc(1000*sizeof(int));
    int* solution=(int*)malloc(depth*sizeof(int));

    pathRecursive(root,res,solution,sum,0,columnSizes,returnSize);

    return res;

}
```