## 092 Reverse Linked List II
[题目地址](https://leetcode.com/problems/reverse-linked-list-ii/description/)
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

struct ListNode* reverseBetween(struct ListNode* head, int m, int n) {
    //边界条件
    if (head==NULL||head->next==NULL||m==n) return head;

    struct ListNode* temp=head;
    struct ListNode* start=head;
    int i=1;

    //找到i=m的位置
    while (i<m&&temp->next!=NULL) {
        start=temp;
        temp=temp->next;
        i++;
    }

    //此时temp在i=m位置，start在i=m-1位置
    //进行下一步翻转，同样用三个指针
    struct ListNode* p=temp;
    struct ListNode* q=temp->next;
    struct ListNode* r;

    while (i<n&&q!=NULL) {
        r=q->next;
        q->next=p;
        p=q;
        q=r;
        i++;
    }

    //最后再首尾相连
    start->next=p;
    temp->next=q;

    //返回结果
    //注意特殊情况
    if (m==1) return p;
    else return head;
}
```