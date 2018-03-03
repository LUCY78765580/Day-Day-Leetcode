## 143 Reorder List
[题目地址](https://leetcode.com/problems/reorder-list/description/)
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


//1）先获取中间节点
//2）获取左右两边，左边不变，右边反转
//3）归并（右边往左边归并）

//这一题与234. Palindrome Linked List（回文链表）很相似，用到链表反转(巧用虚拟表头)，快慢指针等知识
//参考答案：https://leetcode.com/problems/reorder-list/discuss/45026/Clear-C-Solution

//reverseList函数，反转链表
struct ListNode* reverseList(struct ListNode* head) {
    struct ListNode* tail = NULL;

    while (head){
        struct ListNode* temp = head;
        head = head->next;
        temp->next = tail;
        tail = temp;
    }

    return tail;
}

//getMid函数，获取中间节点
//用到快慢指针
struct ListNode* getMid(struct ListNode* head) {
    struct ListNode* fast=head;
    struct ListNode* slow=head;

    while (fast->next&&fast->next->next) {
        slow=slow->next;
        fast=fast->next->next;
    }
    return slow;
}

//归并函数
void merge(struct ListNode* head1, struct ListNode* head2){
    struct ListNode* p = head1;

    //从head2中，抽头结点，往head1中插入
    while (head2) {
        struct ListNode* q = head2->next;
        //插入
        head2->next = p->next;
        p->next = head2;

        //因为之前已经插入了一个，所以p=p->next->next,而不是p=p->next;
        //重新设head2,开启新一轮
        p = p->next->next;
        head2 = q;
    }
}

//主函数
void reorderList(struct ListNode* head) {
    if(head == NULL || head->next == NULL || head->next->next == NULL) return ;

    struct ListNode* mid = getMid(head);
    struct ListNode* secondHalf = mid->next;

    //左边一半
    mid->next = NULL;
    //右边一半
    secondHalf = reverseList(secondHalf);
    //归并
    merge(head, secondHalf);
}
```