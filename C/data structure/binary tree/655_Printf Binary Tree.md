## 655  Print Binary Tree
[题目地址](https://leetcode.com/problems/print-binary-tree/description/)
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

/*题目大意：
按照以下规则在m * n 2D字符串数组中打印二叉树：
1、行号m应该等于给定二叉树的高度。列号n应始终为奇数。

2、根节点的值（以字符串格式）应该放在第一行的中间位置。

3、根节点所在的列和行将把剩余空间分成两部分（左下部分和右下部分）。 在左下角打印左侧子树，
并在右下侧打印右侧子树。 左下部分和右下部分应该具有相同的尺寸。

4、即使一个子树没有，而另一个子树不是，也不需要为无子树打印任何东西，但仍然需要留下与其他子树相同的空间。 但是，如果两棵子树都没有，那么你就不需要为它们留下空间。每个未使用的空间应该包含一个空字符串“”。按照相同的规则打印子树。

*/


//打印二叉树
//思路：先求高度，初始化数组，然后DFS递归即可。在DFS时，需要注意确定左右边界。
//由题目可得数组行和列之间的关系，设行数为m 列数为n，则有n = 2^m - 1
//公式推导过程可以参考：https://leetcode.com/problems/print-binary-tree/discuss/106273/Simple-Python-with-thorough-explanation


int maxDepth(struct TreeNode* root) {
    if (root==NULL) return 0;
    int L=maxDepth(root->left);
    int R=maxDepth(root->right);
    int M=(L>R)?L:R;

    return M+1;
}


//注意res[d][mid]=(char*)malloc(4*sizeof(char))  -->要存的是数字所以是4个
//sprintf函数的使用，将一个格式化字符串，输出到目的字符串中
void pathRecursive(struct TreeNode* root,char*** res,int d,int depth,int left,int right) {
    if (root==NULL || d==depth) return ;

    int mid=(left+right)/2;
    res[d][mid]=(char*)malloc(4*sizeof(char));
    sprintf(res[d][mid],"%d",root->val);

    pathRecursive(root->left,res,d+1,depth,left,mid-1);
    pathRecursive(root->right,res,d+1,depth,mid+1,right);
}


//由图，有多少行就有多少 *returnSize
//columnSizes可以看作一行，depth列
//columnSizes[0][i]记录每一列的长度，即为width
char*** printTree(struct TreeNode* root,int** columnSizes,int* returnSize) {
    int depth=maxDepth(root);
    int width=pow(2,depth)-1;

    *returnSize=depth;
    char*** res=(char***)malloc(depth*sizeof(char**));
    columnSizes[0]=(int*)malloc(depth*sizeof(int));

    for (int i=0;i<depth;i++) {
        columnSizes[0][i]=width;
        res[i]=(char**)malloc(width*sizeof(char*));
        for (int j=0;j<width;j++) {
            res[i][j]="";
        }
    }

    pathRecursive(root,res,0,depth,0,width-1);
    return res;
}
```