## 501  Find Mode in Binary Search Tree
[题目地址](https://leetcode.com/problems/find-mode-in-binary-search-tree/description/)
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


//找二叉搜索树的众数

/*
BST中左根右结点之间的关系是小于等于的，众数就是出现最多次的数字，可以有多个。

1、比较直接的方法是构造一个哈希表，记录数字和其出现次数之前的映射，同时变量mx记录当前最多的次数值。
遍历完树之后，根据mx值就能把对应的元素找出来。用随意一种遍历方式都可以。

2、根据BST的性质求解，二分搜索树中序遍历出来的结果是有序的，只要比较前后两个元素是否相等，就等统计出现某个元素出现的次数。
因为相同的元素肯定是都在一起的，结点变量pre记录上一个遍历到的结点，然后mx还是记录最大的次数，cnt来计数当前元素出现的个数。
  1）中序遍历的时如果pre不为空，说明当前不是第一个结点，我们和之前一个结点   值比较，如果相等cnt自增1，如果不等cnt重置1。
  2）如果此时cnt大于了mx，那么我们清空结果res，并把当前结点值加入结果res；
     如果cnt等于mx，那我们直接将当前结点值加入结果res，然后mx赋值为cnt。
  3）最后我们要把pre更新为当前结点

参考资料：https://www.cnblogs.com/grandyang/p/6436150.html
*/


//注意啊：这里的众数可以有很多个的！！！
//方法一：使用hash_map+中序遍历

//方法二：利用BST性质+中序遍历(递归/非递归)数组+计数器
//递归函数(也可以构建非递归的helper)

void helper(struct TreeNode* root,int** array,int* len) {
    if (root==NULL) return ;

    helper(root->left,array,len);

    *array=(int*)realloc(*array,(*len+1)*sizeof(int));
    (*array)[(*len)++]=root->val;

    helper(root->right,array,len);
}

int* findMode(struct TreeNode* root,int* returnSize) {
    int* res=(int*)calloc(0,sizeof(int));
    int* array=(int*)calloc(0,sizeof(int));

    int len=0;int maxSum=0;
    int i=0;int j=0;

    //首先递归BST，将数据存入array数组中
    //序列由小到大排序的序列，大小为len
    helper(root,&array,&len);

    while (i<len) {
        //j从1开始遍历
        for (j=i+1;j<len;j++) {
            if (array[i]!=array[j]) break;
        }

        int count=j-i;
        //count==maxSum,说明可能有多个众数
        //这时需要调整res，*returnSize的值（多加一个）
        if (count==maxSum) {
            res=(int*)realloc(res,(*returnSize+1)*sizeof(int));
            res[*returnSize]=array[i];
            (*returnSize)++;
        }
        //如果count>maxSum,说明众数是后面那一个
        //maxSum=count,res从头开始（原来的不算了）,*returnSize=1
        else if (count>maxSum) {
            maxSum=count;
            *returnSize=1;
            res=(int*)calloc(1,sizeof(int));
            res[0]=array[i];
        }

        i=j;//i= i+count =i+j-i = j
    }

    return res;
}


//方法三：利用BST性质+中序遍历（迭代）+计数器
//这里的好处是不用再定义一个数组，同时不用递归
int* findMode(struct TreeNode* root,int* returnSize) {
    if (root==NULL) return ;

    struct TreeNode** stack=(struct TreeNode**)malloc(1000*sizeof(struct TreeNode*));
    int* res=(int*)calloc(0,sizeof(int));

    int len=0;int count=1;int maxSum=0;
    struct TreeNode* temp=NULL;

    while (len>0 || root!=NULL) {
        if (root!=NULL) {
            stack[len++]=root;
            root=root->left;
        }
        else {
            //这个else一定要加上的
            root=stack[--len];
            //注意temp=root放的位置
            //这里的意思是，这次的root和temp（就是上次出来的root）做比较
            if (temp) count=(root->val==temp->val)?count+1:1;
            if (count==maxSum) {
                res=(int*)realloc(res,(*returnSize+1)*sizeof(int));
                res[(*returnSize)++]=root->val;
            }
            else if (count>maxSum) {
                maxSum=count;
                *returnSize=1;
                res=(int*)calloc(1,sizeof(int));
                res[0]=root->val;
            }
            temp=root;
            root=root->right;
        }
    }

    return res;
}
```