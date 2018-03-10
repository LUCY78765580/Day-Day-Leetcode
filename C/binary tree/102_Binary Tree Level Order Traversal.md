## 102  Binary Tree Level Order Traversal
[题目地址](https://leetcode.com/problems/binary-tree-level-order-traversal/description/)
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

//二叉树的层序遍历
//层序遍历大概有两种思路：1、用队列辅助，记录每一层节点数  2、用递归方式，记录层级
//参考博客：http://blog.csdn.net/crazy1235/article/details/51507485

/*
//方法一：(DFS深度优先)  使用递归，看不懂的>_<
//参考答案：https://leetcode.com/problems/binary-tree-level-order-traversal/discuss/33585/

//关键：记录当前节点所在那一层（记录层级）
//策略：使用递归，变量depth每一次+1

void levelRecursive(struct TreeNode* root,int depth,int*** res,int** columnSizes,int* returnSize) {
    if (!root) return ;
    if (*returnSize<depth+1) {
        *returnSize=depth+1;

        *res=(int**)realloc(*res,(*returnSize)*sizeof(int*));
        (*res)[depth]=NULL;

        *columnSizes=(int*)realloc(*columnSizes,(*returnSize)*sizeof(int));
        (*columnSizes)[depth]=0;
    }

    (*res)[depth]=(int**)realloc((*res)[depth],((*columnSizes)[depth]+1)*sizeof(int*));
    (*res)[depth][(*columnSizes)[depth]]=root->val;
    ++(*columnSizes)[depth];
    //相当于(*columnSizes)[depth]=(*columnSizes)[depth]+1;

    levelRecursive(root->left,depth+1,res,columnSizes,returnSize);
    levelRecursive(root->right,depth+1,res,columnSizes,returnSize);
}

int** levelOrder(struct TreeNode* root, int** columnSizes, int* returnSize) {
    int** res;

    res=NULL;
    *returnSize=0;

    levelRecursive(root,0,&res,columnSizes,returnSize);
    return res;
}
*/




//方法二：(BFS广度优先)  使用队列queue (懂了哈哈)
//参考答案：https://leetcode.com/problems/binary-tree-level-order-traversal/discuss/33569/

//思路：与普通层序遍历差不多。先把根节点压入，弹出根节点，并判断是否有左右儿子节点再压入
//关键：记录每层节点个数
//策略：使用队列，cur_size和next_size分别记录当层节点数和下层节点数

int maxDepth(struct TreeNode* root) {
    if (root==NULL) return 0;
    int L=maxDepth(root->left);
    int R=maxDepth(root->right);
    int M=(L>R)?L:R;

    return M+1;
}


int** levelOrder(struct TreeNode* root, int** columnSizes, int* returnSize) {
    if (root==NULL) return NULL;
    int depth=*returnSize=maxDepth(root);

    int** res=(int**)calloc(depth,sizeof(int*));
    *columnSizes=(int*)calloc(depth,sizeof(int));

    //构造队列并把根节点压入
    struct TreeNode* queue[5000];
    int front=0,rear=0;
    queue[rear++]=root;
    int cur_size=1;int next_size=0;int size=0;

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

        (*columnSizes)[size++]=cur_size;

        cur_size=next_size;
        if (cur_size==0) break;
        next_size=0;
    }

    return res;
}
```