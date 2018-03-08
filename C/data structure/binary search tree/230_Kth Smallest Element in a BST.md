## 230  Kth Smallest Element in a BST
[题目地址](https://leetcode.com/problems/kth-smallest-element-in-a-bst/description/)
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

//找BST第K个最小的数
//网友博客讲的很好：https://www.cnblogs.com/grandyang/p/4620012.html


//方法一：中序遍历(非递归)+加上一个计数器
//二叉树中序遍历可以参考：https://github.com/LUCY78765580/Day-Day-Leetcode/blob/master/C/data%20structure/binary%20tree/094_Binary%20Tree%20Inorder%20Traversal.md

int kthSmallest(struct TreeNode* root, int k) {
    int total=64;
    struct TreeNode** stack=(struct TreeNode**)malloc(total*sizeof(struct TreeNode*));
    int len=0;
    int count=0;

    while (len>0 || root!=NULL) {
        if (root!=NULL) {
            stack[len++]=root;
            root=root->left;
        }
        else {
            root=stack[--len];
            count++;
            if (count==k) return root->val;
            root=root->right;
        }

        //检查内存并重新分配
        if (len==total) {
            total<<=1;
            stack=(struct TreeNode**)realloc(stack,total*sizeof(struct TreeNode*));
        }
    }

    return 0;
}



//方法二：中序遍历(递归)+一个计数器
//稍难理解>_<
int kthSmallestDFS(struct TreeNode* root,int* k) {
    if (root==NULL) return -1;

    int val=kthSmallestDFS(root->left,k);
    if (*k==0) return val;
    if (--(*k)==0) return root->val;

    return kthSmallestDFS(root->right,k);
}

int kthSmallest(struct TreeNode* root, int k) {
    return kthSmallestDFS(root,&k);
}




//方法三：递归+分治
/*
由于BST的性质，我们可以快速定位出第k小的元素是在左子树还是右子树,首先计算出左子树的结点个数总和cnt

1、如果k小于等于左子树结点总和cnt，说明第k小的元素在左子树中，直接对左子结点调用递归即可。
2、如果k大于cnt+1，说明目标值在右子树中，对右子结点调用递归函数，注意此时的k应为k-cnt-1，应为已经减少了cnt+1个结点。
3、如果k正好等于cnt+1，说明当前结点即为所求，返回当前结点值即可，参见代码如下：
*/

//定义一个count函数
int count(struct TreeNode* node) {
    if (node==NULL) return 0;
    return 1+count(node->left)+count(node->right);
}

int kthSmallest(struct TreeNode* root, int k) {
    int size=count(root->left);
    if (k<=size)
        return kthSmallest(root->left,k);
    else if (k>size+1)
        return kthSmallest(root->right,k-size-1);

    //k==size+1,即k正好处于root节点上
    return root->val;
}




//方法四：FOLLOW UP 上面方法的改进
/*
这道题的Follow up中说假设该BST被修改的很频繁，而且查找第k小元素的操作也很频繁，问我们如何优化。

其实最好的方法还是像上面的解法那样利用分治法来快速定位目标所在的位置.
但是每个递归都遍历左子树所有结点来计算个数的操作并不高效，所以我们应该修改原树结点的结构,
使其保存包括当前结点和其左右子树所有结点的个数，这样我们使用的时候就可以快速得到任何左子树结点总数来帮我们快速定位目标值了。

定义了新结点结构体，然后就要生成新树，还是用递归的方法生成新树.
注意生成的结点的count值要累加其左右子结点的count值。然后在求第k小元素的函数中，我们先生成新的树，然后调用递归函数。在递归函数中，不能直接访问左子结点的count值，因为左子节结点不一定存在，所以我们先判断，如果左子结点存在的话，那么跟上面解法的操作相同。如果不存在的话，当此时k为1的时候，直接返回当前结点值，否则就对右子结点调用递归函数，k自减1

*/

/*
// java代码

class Solution {
public:
    struct MyTreeNode {
        int val;
        int count;
        MyTreeNode *left;
        MyTreeNode *right;
        MyTreeNode(int x) : val(x), count(1), left(NULL), right(NULL) {}
    };

    MyTreeNode* build(TreeNode* root) {
        if (!root) return NULL;
        MyTreeNode *node = new MyTreeNode(root->val);
        node->left = build(root->left);
        node->right = build(root->right);
        if (node->left) node->count += node->left->count;
        if (node->right) node->count += node->right->count;
        return node;
    }

    int kthSmallest(TreeNode* root, int k) {
        MyTreeNode *node = build(root);
        return helper(node, k);
    }

    int helper(MyTreeNode* node, int k) {
        if (node->left) {
            int cnt = node->left->count;
            if (k <= cnt) {
                return helper(node->left, k);
            } else if (k > cnt + 1) {
                return helper(node->right, k - 1 - cnt);
            }
            return node->val;
        } else {
            if (k == 1) return node->val;
            return helper(node->right, k - 1);
        }
    }
};
*/
```