## 103  Binary Tree Zigzag Level Order Traversal
[题目地址](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/description/)
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


//二叉树的锯齿层序遍历

//基本方法还是与前面层序遍历类似
/*主要技巧：
增加一个标记flag,巧用 按位异或，
实现flag=0和flag=1之间的转换，
从而控制二叉树遍历（数据存储）方向
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


int** zigzagLevelOrder(struct TreeNode* root, int** columnSizes, int* returnSize) {
    if (root==NULL) return NULL;
    int depth=*returnSize=maxDepth(root);

    int** res=(int**)calloc(depth,sizeof(int*));
    *columnSizes=(int*)calloc(depth,sizeof(int));

    int front=0,rear=0;
    struct TreeNode* queue[5000];
    queue[rear++]=root;

    int cur_size=1;int next_size=0;int size=0;
    int flag=0;

    while (front<rear) {
        res[size]=(int*)malloc(1000*sizeof(int));

        //最最开始flag等于0,令j为0,j++         (flag=0,j=0          从res[size]最左边记录)
        //下一层flag变为1,令j为cur_size-1,j--  (flag=1,j=cur_size-1 从res[size]最右边记录)
        //因为cur_size已经由上一步得到，即为小数组的长度
        //由此类推
        int j=(flag)?cur_size-1:0;
        for (int i=0;i<cur_size&&front<rear;i++) {
            struct TreeNode* temp=queue[front++];

            if (flag==0) {
                res[size][j++]=temp->val;
            } else {
                res[size][j--]=temp->val;
            }

            if (temp->left) {
                queue[rear++]=temp->left;
                next_size++;
            }
            if (temp->right) {
                queue[rear++]=temp->right;
                next_size++;
            }
        }

        (*columnSizes)[size++]=cur_size;
        cur_size=next_size;
        if (cur_size==0) break;
        next_size=0;

        flag ^=1;
        //按位异或，c=a^b  如果ab相同结果c为0，ab不同结果c为1
        //flag^=1相当于 flag=flag^1
        //如果此前的flag是1，令flag为0；反之如果此前的flag为0，令flag为1

        //按位运算，方便实现数的交换
    }

    return res;
}
```