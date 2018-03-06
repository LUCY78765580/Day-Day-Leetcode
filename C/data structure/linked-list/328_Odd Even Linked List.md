## 328 Odd Even Linked List
[题目地址](https://leetcode.com/problems/odd-even-linked-list/description/)
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

//奇偶链表
//注意：这里的奇偶指的是节点位置，而不是节点值
//偶数与奇数节点分组内部的相对顺序应当与输入保持一致。第一个节点为奇数节点，第二个节点为偶数节点，以此类推。

struct ListNode* oddEvenList(struct ListNode* head) {
    //边界条件
    if (head==NULL || head->next==NULL) return head;

    //初始化，注意标记偶数部分的头为evenHead
    struct ListNode* odd;
    struct ListNode* even;
    struct ListNode* evenHead;
    odd=head;even=head->next;
    evenHead=even;

    while (odd->next && even->next) {
        odd->next=even->next;
        odd=odd->next;
        even->next=odd->next;
        even=even->next;
    }

    //最后将奇数部分尾巴odd，与偶数部分的头evenHead相连
    odd->next=evenHead;
    return head;
}
```