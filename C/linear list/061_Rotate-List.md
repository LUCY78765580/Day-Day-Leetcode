## 061 Rotate List
[题目地址](https://leetcode.com/problems/rotate-list/description/)
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

//方法一：闭环中循环
//这题其实和求链表倒数第 k 个节点具有异曲同工之处。
//找到倒数第k个节点的前驱(p)，将倒数第k个节点及其之后的节点摘下(np)，链接到头结点之前即可。
//要注意的是，k 的大小可能大于链表的长度，这时应该先计算链表的长度，再用 k % 链表的长度，得到实际应该右移的位数

struct ListNode* rotateRight(struct ListNode* head, int k) {
    if (!head) return NULL;
    struct ListNode* p;
    struct ListNode* np;

    //求链表长度
    int len=1;p=head;
    while (p->next) {
        p=p->next;
        len++;
    }

    //此时p已经到达链表尾部
    //先形成一个闭环，然后处理k(注意k比len大的时候)
    p->next=head;
    for (int i=0; i<(len-k%len); i++)
        p=p->next;

    np=p->next;
    p->next=NULL;
    return np;
}



//方法二：使用快慢指针
//本质和方法一是一样的
struct ListNode* rotateRight(struct ListNode* head, int k) {
    if (!head) return NULL;

    struct ListNode* slow;
    struct ListNode* fast;
    slow=fast=head;

    //快指针遍历获得链表长度
    int len=1;
    while(fast->next) {
        fast=fast->next;
        len++;
    }

    //慢指针指到新链表，前一位置
    for (int i=len-k%len;i>1;i--)
        slow=slow->next;

    //进行翻转
    fast->next=head;
    head=slow->next;
    slow->next=NULL;
    return head;
}
```