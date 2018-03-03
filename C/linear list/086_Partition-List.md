## 86 Partition List
[题目地址](https://leetcode.com/problems/partition-list/description/)
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

//这个题和前面的Odd Even有一点相似
//要点：虚拟表头的构造
//同时p1,p2分别为两新表中最后节点

struct ListNode* partition(struct ListNode* head, int x) {
    if (head==NULL || head->next==NULL) return head;

    struct ListNode *head1,*head2;
    struct ListNode *p1,*p2;
    head1=malloc(sizeof(struct ListNode));
    head2=malloc(sizeof(struct ListNode));
    p1=head1;p2=head2;

    while (head) {
        if (head->val<x) {
            p1->next=head;
            p1=head;
        }
        else {
            p2->next=head;
            p2=head;
        }
        head=head->next;
    }

    //注意最后的p1->next指向的是head2->next,而不是head2
    //同理，返回head1->next,而不是head1
    //严格来讲应该将head1、head2，分别free掉的
    p2->next=NULL;
    p1->next=head2->next;
    return head1->next;
}
```