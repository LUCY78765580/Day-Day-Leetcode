## 257  Binary Tree Paths
[题目地址](https://leetcode.com/problems/binary-tree-paths/description/)
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

//求二叉树的路径
//这一题和前面的 [113 Path Sum II](https://leetcode.com/problems/path-sum-ii/description/)非常类似
//注意这里不需要求columnSize了


/*sprintf函数：
sprintf()函数用于将格式化的数据写入字符串，
 原型：int sprintf(char *str, char * format [, argument, ...]);
str为要写入的字符串；format为格式化字符串；argument为变量(变量可以为多个)

1、sprintf的作用是将一个格式化的字符串输出到一个目的字符串中，而printf是将一个格式化的字符串输出到屏幕。
sprintf的第一个参数应该是目的字符串，如果不指定这个参数，执行过程中出现 "该程序产生非法操作,即将被关闭...."的提示。
2、C语言对数组进行操作时并不检测数组的长度，如果str的长度不够，sprintf()很容易造成缓冲区溢出

参考博客：http://c.biancheng.net/cpp/html/295.html
*/


//memcpy，改成copy
//result+j为每一次开始写入的地址
//最后j-2，减去2是为了去掉最后的"->"
void copy(char* result,int* solution,int len) {
    int j=0;
    for(int i=0;i<len;i++) {
        j+=sprintf(result+j,"%d->",solution[i]);
    }

    result[j-2]='\0';
}

//递归函数
//不太明白为什么每一次都要(*len)--
void pathRecursive(struct TreeNode* root,char** res,int* solution,int* len,int* count) {
    if (root==NULL) return ;

    solution[(*len)++]=root->val;
    //注意 root-to-leaf paths
    if (root->left==NULL&&root->right==NULL) {
        res[*count]=(char*)malloc(1000*sizeof(char));
        copy(res[*count],solution,*len);
        (*count)++;
        (*len)--;
        return ;
    }

    pathRecursive(root->left,res,solution,len,count);
    pathRecursive(root->right,res,solution,len,count);
    (*len)--;
}

//主函数
//*returnSize（*count）是整个大数组res的最终长度
//*len是递归过程中，小数组solution的长度
char** binaryTreePaths(struct TreeNode* root,int* returnSize) {
    if (root==NULL) return NULL;
    *returnSize=0;

    char** res=(char**)malloc(1000*sizeof(char*));
    int* solution=(int*)malloc(1000*sizeof(int));
    int* len=(int*)malloc(sizeof(int));
    *len = 0;
    //注意这里*len的定义,len长度仅仅为1
    //用solution记录经过的数据
    //solution长度不能再用maxDepth,因为还要包含"->"这些东西

    pathRecursive(root,res,solution,len,returnSize);
    return res;
}
```