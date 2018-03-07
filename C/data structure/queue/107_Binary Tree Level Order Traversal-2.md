## 107  Binary Tree Level Order Traversal II
[题目地址](https://leetcode.com/problems/binary-tree-level-order-traversal-ii/description/)
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


//二叉树的层序遍历(由底部到顶部)

/*
看起来挺费劲，其实一点也不难，只需在size上做一点手脚就行

前面已经得size=depth-1,那么可以按顺序，由上到下遍历，然后从相反方向存储就行，每一次size--

*/

int maxDepth(struct TreeNode* root) {
    if (root==NULL) return 0;
    else {
        int L=maxDepth(root->left);
        int R=maxDepth(root->right);
        int M=(L>R)?L:R;

        return M+1;
    }
}

int** levelOrderBottom(struct TreeNode* root, int** columnSizes, int* returnSize) {
    if (root==NULL) return NULL;
    int depth=*returnSize=maxDepth(root);

    int** res=(int**)calloc(depth,sizeof(int*));
    *columnSizes=(int*)calloc(depth,sizeof(int));

    struct TreeNode* queue[5000];
    int front=0,rear=0;
    queue[rear++]=root;
    int cur_size=1;int next_size=0;int size=depth-1; //这个地方由size=0变成size=depth-1

    //size  代表res中第size个小数组
    //(*columnSizes)[size]代表第size个小数组长度
    //cur_level  当层节点数
    //next_level 下层节点数

    //若front==rear  队列为空,每加入一个元素rear加一，每删除一个元素front减一
    //若cur_level=0  说明next_size为0,没有左右儿子节点，已经是最后的叶子节点，退出循环
    while (front<rear) {
        res[size]=(int*)malloc(1000*sizeof(int));

        for (int i=0;i<cur_size&&front<rear;i++) {
            struct TreeNode* temp=queue[front++];
            res[size][i]=temp->val;

            if (temp->left) {
                queue[rear++]=temp->left;
                next_size++;
            }
            if (temp->right) {
                queue[rear++]=temp->right;
                next_size++;
            }
        }

        //这个地方做下改动
        (*columnSizes)[size--]=cur_size;

        cur_size=next_size;
        if (cur_size==0) break;
        next_size=0;
    }

    return res;
}
```