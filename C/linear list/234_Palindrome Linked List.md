## 234 Palindrome Linked List
[题目地址](https://leetcode.com/problems/palindrome-linked-list/description/)
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

//可以有以下三种方式
//1、遍历链表记录在数组中：再判断数组是不是一个回文数组，时间复杂度为O（n），但空间复杂度也为O（n），不满足空间复杂度要求。
//2、堆栈：将链表前半段压入栈中，再逐个弹出与链表后半段比较。时间复杂度O（n），但仍然需要n/2的栈空间，空间复杂度为O（n）。
//3、反转链表法：将链表左半段原地翻转，再将前半段、后半段依次比较，判断是否相等，时间复杂度O（n），空间复杂度为O（1）满足题目要求。

//链表翻转可以参考之前 206 Reverse Linked List
//本题采用方法三，并使用快慢指针辅助(这就比较巧妙了)

bool isPalindrome(struct ListNode* head) {
    //初始化，构造快慢指针，构造aux虚拟表头
    struct ListNode* slow=head;
    struct ListNode* fast=head;
    struct ListNode* aux=NULL;

    while (fast&&fast->next) {
        struct ListNode* temp=slow;
        slow=slow->next;
        fast=fast->next->next;
        temp->next=aux;
        aux=temp;
    }
    //当表长为偶数，最后fast为NULL
    //当表长为奇数，最后fast不为NULL，同时slow需要再走一步才到右边
    if (fast) slow=slow->next;

    //比较两边，左边以aux为头指针，右边以slow为头指针
    while (slow&&aux->val==slow->val) {
        aux=aux->next;
        slow=slow->next;
    }

    //走到最后，slow不剩下，slow==NULL,则说明两者比较完全相等
    return slow==NULL;
}
```

