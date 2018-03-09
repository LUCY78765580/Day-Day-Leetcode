## 530  Minimum Absolute Difference in BST
[题目地址](https://leetcode.com/problems/minimum-absolute-difference-in-bst/description/)
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

//BST最小绝对值差
//这里的方法与后面的 501 Find Mode in Binary Search Tree 很相似

/*
注意题干中提到的所有值都为非负数,所以最小差值，一定诞生在序列的相邻两个数之间 temp[i]-temp[i-1]
这个题目的关键在于BST中序遍历，得到数值由小到大排列的序列,然后当前节点值和之前节点值求绝对差并更新结果res
*/


//方法一：中序遍历（递归形式）
void inorder(struct TreeNode* root,int* temp,int* size) {
    if (root==NULL) return ;

    inorder(root->left,temp,size);
    temp[(*size)++]=root->val;
    inorder(root->right,temp,size);
}

int getMinimunDifference(struct TreeNode* root) {
    int* temp=(int*)malloc(5000*sizeof(int));
    int size=0;
    inorder(root,temp,size);

    int min=INT_MAX;
    for (int i=1;i<size;i++) {
        min=min<abs(temp[i]-temp[i-1])?min:abs(temp[i]-temp[i-1]);
    }

    free(temp);
    return min;
}


//方法二：中序遍历（迭代形式）
int getMinimumDifference(struct TreeNode* root) {
    struct TreeNode** stack=(struct TreeNode**)malloc(5000*sizeof(struct TreeNode*));
    struct TreeNode* temp;
    int len=0;int min;
    int count=1;

    while (len>0 || root!=NULL) {
        if (root) {
            stack[len++]=root;
            root=root->left;
        }
        else {
            //注意第一次temp是为NULL，不能相减
            //所以添加count这个计数器，并设置相关条件
            root=stack[--len];
            if (count==1)
                min=INT_MAX;
            else
                min=min<abs(root->val-temp->val)?min:abs(root->val-temp->val);

            temp=root;
            root=root->right;
            count++;
        }
    }

    return min;
}



//方法三：不用中序遍历，设定一个low/high上下界，用先序遍历（递归）
//有些费脑，参考：https://www.cnblogs.com/grandyang/p/6540165.html

```