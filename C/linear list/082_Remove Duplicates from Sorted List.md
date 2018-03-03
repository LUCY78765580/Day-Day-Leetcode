## 082  Remove Duplicates from Sorted List II
[题目地址](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/description/)
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


//方法一：巧用虚拟表头
struct ListNode* deleteDuplicates(struct ListNode* head) {
    struct ListNode* p;
    struct ListNode* q;
    struct ListNode* newHead;
    struct ListNode* temp;

    if (head==NULL) return NULL;

    //创建一个虚拟表头newHead
    newHead=malloc(sizeof(struct ListNode));
    newHead->next=head;

    p=head;
    q=newHead;
    while (p) {
        while (p->next&&p->next->val==p->val) {
            p=p->next;
        }

        //q->next不等于p，说明有重复
        //直接q->next=p->next,将前重复的全部略过（删除）
        if (q->next==p) q=q->next;
        else    q->next=p->next;

        p=p->next;
    }

    //最后将虚拟表头free掉
    temp=newHead;
    newHead=temp->next;
    free(temp);
    return newHead;
}



//方法二：使用递归
struct ListNode* deleteDuplicates(struct ListNode* head) {
    if (head==NULL) return NULL;

    //如果1,2位重复，设置循环，使得head=head->next
    //一直到最后，从head下一个开始，即递归返回deleteDuplicates(head->next)
    if (head->next&&head->next->val==head->val) {
        while (head->next&&head->next->val==head->val) {
            head=head->next;
        }
        return deleteDuplicates(head->next);
    }

    //如果1,2位不重复，递归实现剩下的head->next
    else {
        head->next=deleteDuplicates(head->next);
    }

    return head;
}
```