## 203 Remove Linked List Elements
[题目地址](https://leetcode.com/problems/remove-linked-list-elements/description/)
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


//非常简单的题目，应用到链表的删除操作
//方法一：一般方法（分别处理val在首位和在中间的情况）
struct ListNode* removeElements(struct ListNode* head, int val) {
    if (head==NULL) return NULL;
    struct ListNode* p;
    struct ListNode* temp;

    //如果val是首位
    while (head&&head->val==val) {
        temp=head;
        head=head->next;
        free(temp);
    }

    //如果val不是首位
    p=head;
    while (p&&p->next) {
        if (p->next->val==val) {
            temp=p->next;
            p->next=temp->next;
            free(temp);
        }
        else p=p->next;
    }

    return head;
}

// 方法二：使用递归
struct ListNode* removeElements(struct ListNode* head, int val) {
    //这一句不可省略（因为要往下递归）
    if (head==NULL) return NULL;

    head->next=removeElements(head->next,val);
    if (head->val==val)
        return head->next;
    else
        return head;
}
```