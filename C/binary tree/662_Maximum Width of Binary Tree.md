## 662  Maximum Width of Binary Tree
[题目地址](https://leetcode.com/problems/maximum-width-of-binary-tree/description/)
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

//二叉树的最大宽度

/*
//一般求二叉树宽度方法如下
//所谓二叉树的宽度是指：二叉树各层节点个数的最大值。
int widthOfBinaryTree(struct TreeNode* root) {
    if (root==NULL) return 0;

    struct TreeNode* queue[5000];
    int front=0;int rear=0;
    queue[rear++]=root;
    int cur_size=1;int next_size=0;int maxWidth=1;

    while (true) {
        for (int i=0;i<cur_size&&front<rear;i++) {
            struct TreeNode* temp=queue[front++];

            if (temp->left) {
                queue[rear++]=temp->left;
                next_size++;
            }
            if (temp->right) {
                queue[rear++]=temp->right;
                next_size++;
            }
        }

        cur_size=next_size;
        maxWidth=(cur_size>maxWidth)? cur_size:maxWidth;
        if (cur_size==0) break;
        next_size=0;
    }

    return maxWidth;
}
*/


//但是本题中，where the null nodes between the end-nodes are also counted into the length calculation.
/*
这里的最大宽度不是满树的时候的最大宽度，如果是那样的话，肯定是最后一层的结点数最多。
最大宽度应该是两个存在的结点中间可容纳的总的结点个数，中间的结点可以为空。
那么其实只要我们知道了每一层中最左边和最右边的结点的位置，我们就可以算出这一层的宽度了。
所以这道题的关键就是要记录每一层中最左边结点的位置
*/
//参考博客：http://www.cnblogs.com/grandyang/p/7538821.html


//BFS层序遍历，使用队列辅助
//有bug，得不到正确答案？？

int widthOfBinaryTree(struct TreeNode* root) {
    if (root==NULL) return 0;

    struct TreeNode* queue[5000];
    int front=0;int rear=0;
    queue[rear++]=root;

    int queuePos[5000];
    int index=0;
    queuePos[index++]=1;

    int start=1;int end=1;
    int cur_size=1;int next_size=0;int maxWidth=cur_size;

    while (front<rear) {
        for (int i=0;i<cur_size;i++) {
            struct TreeNode* temp=queue[front++];
            end=queuePos[--index];

            if (temp->left) {
                queue[rear++]=temp->left;
                queuePos[index++]=end*2;
                next_size++;
            }
            if (temp->right) {
                queue[rear++]=temp->right;
                queuePos[index++]=end*2+1;
                next_size++;
            }
        }

        maxWidth=maxWidth > (end-start+1)? maxWidth:(end-start+1);

        cur_size=next_size;
        if (cur_size==0) break;
        next_size=0;
        start=(index<=0) ? 1:queuePos[index-1];

    }

    return maxWidth;
}
```