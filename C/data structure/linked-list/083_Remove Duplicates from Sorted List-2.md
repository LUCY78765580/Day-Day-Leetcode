## 083  Remove Duplicates from Sorted List
[题目地址](https://leetcode.com/problems/remove-duplicates-from-sorted-list/description/)
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

//这个题很简单，因为链表已经有序排列，遇到重复的，用一个temp指针free掉即可
struct ListNode* deleteDuplicates(struct ListNode* head) {
    struct ListNode* p=head;
    struct ListNode* temp;

    while (p&&p->next) {
        if (p->next->val==p->val) {
            temp=p->next;
            p->next=temp->next;
            free(temp);
        }
        else
            p=p->next;
    }

    return head;
}
```