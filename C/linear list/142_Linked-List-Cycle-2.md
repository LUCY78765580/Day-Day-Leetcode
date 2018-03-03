## 142 Linked List Cycle II
[题目地址](https://leetcode.com/problems/linked-list-cycle-ii/description/)
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

//带环链表，与前面类似
//规律：头指针(head)到连接点(node)的距离 = 碰撞点(slow==fast时)到连接点(node)的距离

struct ListNode *detectCycle(struct ListNode *head) {
    if (!head) return NULL;

    struct ListNode* slow;
    struct ListNode* fast;
    slow=fast=head;

    while (fast->next&&fast->next->next) {
        slow=slow->next;
        fast=fast->next->next;

        //开始时slow和fast，步长分别为1、2
        //slow==fast时，两者碰头相遇
        if (slow==fast) {
            //这时候将slow指针拨到head这里
            //slow和fast步长都变成1，看最终是否聚集在一点上(改点就是节点node)
            slow=head;
            while (slow!=fast) {
                slow=slow->next;
                fast=fast->next;
            }
            return slow;
            break;
        }

    }

    return NULL;
}
```