## 109  Convert Sorted List to Binary Search Tree
[题目地址](https://leetcode.com/problems/convert-sorted-list-to-binary-search-tree/description/)
<br>
<br>
<br>

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */

//将排序链表转化为BST
//方法跟前面的 108 Convert Sorted Array to Binary Search Tree类似


//方法一：递归+二分
//用while循环获取链表长度

//特别注意建树的顺序： l=递归() --> 创建root -->  root.left=l -->  root.right=递归另一半
struct ListNode* current;
struct TreeNode* toBST(int start,int end) {
    if (start>end) return NULL;

    int mid=(start+end)/2;
    struct TreeNode* L=toBST(start,mid-1);
    struct TreeNode* root=malloc(sizeof(struct TreeNode));
    root->val=current->val;

    root->left=L;
    current=current->next;

    struct TreeNode* R=toBST(mid+1,end);
    root->right=R;
    return root;
}

struct TreeNode* sortedListToBST(struct ListNode* head) {
    if (head==NULL) return NULL;
    current=head;

    int numsSize=0;
    while (head) {
        head=head->next;
        numsSize++;
    }

    return toBST(0,numsSize-1);
}



//方法二：同样是递归+二分，使用快慢指针求取中点
struct TreeNode* toBST(struct ListNode* head,struct ListNode* tail) {
    if (head==tail) return NULL;
    struct ListNode* slow=head;
    struct ListNode* fast=head;

    while (fast!=tail&&fast->next!=tail) {
        fast=fast->next->next;
        slow=slow->next;
    }

    struct TreeNode* root=malloc(sizeof(struct TreeNode));
    root->val=slow->val;
    root->left=toBST(head,slow);
    root->right=toBST(slow->next,tail);

    return root;
}


struct TreeNode* sortedListToBST(struct ListNode* head) {
    if (head==NULL) return NULL;
    return toBST(head,NULL);
}
```