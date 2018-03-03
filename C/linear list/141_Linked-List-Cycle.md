## 141 Linked List Cycle
[题目地址](https://leetcode.com/problems/linked-list-cycle/description/)
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

//带环链表
//类似于圆形操场，跑得快的那一个最终会追上跑的慢的
//同理判断链表中是否有环，可以用快慢指针
//快慢指针步长分别设为2和1，如果两指针最终能相遇，那么肯定有环（反之没有）

bool hasCycle(struct ListNode *head) {
    if (head==NULL||head->next==NULL)
        return false;

    struct ListNode* slow;
    struct ListNode* fast;
    slow=head;fast=head->next;

    while (slow!=fast) {
        if (fast==NULL||fast->next==NULL)
            return false;
        slow=slow->next;
        fast=fast->next->next;
    }
    return true;
}
```
