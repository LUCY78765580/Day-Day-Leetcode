## 206 Reverse Linked List
[题目地址](https://leetcode.com/problems/reverse-linked-list/description/)
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

//法一：三个指针
struct ListNode* reverseList(struct ListNode* head) {
    struct ListNode* p;
    struct ListNode* q;
    struct ListNode* r;

    if (head==NULL||head->next==NULL)
        return head;

    p=head;
    q=head->next;
    head->next=NULL;

    while(q){
        r=q->next;
        q->next=p;
        p=q;
        q=r;
    }
    head=p;
    return head;
}


//法二：使用递归
/*可以先假设B->C->D已经反转好，已经成为了D->C->B,那么接下来要做的事情就是将D->C->B看成一个整体，让这个整体的next指向A，所以问题转化了反转B->C->D。
那么，可以先假设C->D已经反转好，已经成为了D->C,那么接下来要做的事情就是将D->C看成一个整体，让这个整体的next指向B，所以问题转化了反转C->D。
那么，可以先假设D(其实是D->NULL)已经反转好，已经成为了D(其实是head->D),那么接下来要做的事情就是将D(其实是head->D)看成一个整体，让这个整体的next指向C，所以问题转化了反转D
*/
struct ListNode* reverseList(struct ListNode* head) {
    if (head==NULL||head->next==NULL) return head;
    struct ListNode* p;
    struct ListNode* newhead;

    p=head->next;
    head->next=NULL;

    newhead=reverseList(p);
    p->next=head;

    return newhead;
}
```