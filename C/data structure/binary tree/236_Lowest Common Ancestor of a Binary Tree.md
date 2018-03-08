## 236  Lowest Common Ancestor of a Binary Tree
[题目地址](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/description/)
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

//求二叉树最近公共祖先
//"LCA"问题
//参考博客（讲的非常好了）：https://www.hrwhisper.me/algorithm-lowest-common-ancestor-of-a-binary-tree/#comment-8354




//方法一：如果是二叉搜索树（BST）
/*
因为二叉搜索树已经排序好了,很简单的思路就是看两个值在root的哪边：
1、两个值都在左边，则LCA在左边
2、两个值都在右边，则LCA在右边
3、一个在左一个在右，则说明LCA就是当前的root节点。
*/
/*
总：if (p->val > q->val) return lowestCommonAncestor(root,q,p);
1、if (root==NULL || root->val >= p->val || root->val <= q->val) return root;
2、if (root->val > q->val) return lowestCommonAncestor(root->left,p,q);
3、if (root->val < p->val) return lowestCommonAncestor(root->right,p,q);
*/
struct TreeNode* lowestCommonAncestor(struct TreeNode* root, struct TreeNode* p, struct TreeNode* q) {
    if (p->val > q->val) return lowestCommonAncestor(root,q,p);
    if (root->val > q->val)
        return lowestCommonAncestor(root->left,p,q);
    else if (root->val < p->val)
        return lowestCommonAncestor(root->right,p,q);
    else
        return root;
}


//方法二：对于普通二叉树，可以用dfs查找p,q的路径，找到他们第一个分叉口，则为LCA
//但是采用自上而下的方式更优雅，参考博客：http://articles.leetcode.com/2011/07/lowest-common-ancestor-of-a-binary-tree-part-i.html

/*
Using a bottom-up approach, we can improve over the top-down approach by avoiding traversing the same nodes over and over again.

We traverse from the bottom, and once we reach a node which matches one of the two nodes, we pass it up to its parent. The parent would then test its left and right subtree if each contain one of the two nodes. If yes, then the parent must be the LCA and we pass its parent up to the root. If not, we pass the lower node which contains either one of the two nodes (if the left or right subtree contains either p or q), or NULL (if both the left and right subtree does not contain either p or q) up.

Sounds complicated? Surprisingly the code appears to be much simpler than the top-down one.
*/

/*
使用自下而上的方法，我们可以通过避免遍历相同的节点来改进自顶向下的方法。
1、从底部开始遍历，一旦我们到达与两个节点中的一个相匹配的节点，我们就将它传递给它的父节点。
2、如果父节点包含两个节点中的一个，父节点将测试其左侧和右侧子树。
3、如果是，那么父母必须是LCA，并且我们将其父母传递给根。
如果不是，则传递包含两个节点之一（如果左侧或右侧子树包含p或q）或者NULL（如果左侧和右侧子树都不包含p或q）的较低节点。

听起来很复杂？ 令人惊讶的是，代码似乎比自上而下的代码简单得多。
*/

/*思路：
从根节点开始遍历，如果node1和node2中的任一个和root匹配，那么root就是最低公共祖先。
如果都不匹配，则分别递归左、右子树，如果有一个 节点出现在左子树，并且另一个节点出现在右子树，则root就是最低公共祖先.
如果两个节点都出现在左子树，则说明最低公共祖先在左子树中，否则在右子树。
*/

struct TreeNode* lowestCommonAncestor(struct TreeNode* root,struct TreeNode* p,struct TreeNode* q) {
    if (root==NULL || root==q || root==p) return root;

    //看左、右子树是否有目标节点，没有则为NULL
    struct TreeNode* left=lowestCommonAncestor(root->left,p,q);
    struct TreeNode* right=lowestCommonAncestor(root->right,p,q);

    //如果左右子树均有目标节点，公共祖先即为root
    if (left&&right) return root;
    return left==NULL?right:left;

    //其它情况需要继续向上标记，显示此节点下面有目标节点
    //left==NULL,说明两个节点都在右子树，即最小公共祖先在右子树，同理
    /*
    if (left!=NULL) return left;
    if (right!=NULL) return right;
    return NULL;
    */
}




//方法三：LCA算法Tarjan求解（并查集+深度优先搜索）。

```