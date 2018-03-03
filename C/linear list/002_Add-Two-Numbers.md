## 002 Add Two Numbers
[题目地址](https://leetcode.com/problems/add-two-numbers/description/)
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

//对应位置相加sum取余数
//两者加起来大于10，carry取1（不大于10carry取0，综合carry=sum/10）,和后面结果相叠加
//这里关键是创建虚拟表头,每次相加后再创建新节点，使用p指针连接
//(l1?l1->val:0)类似表达式使用，使得编程变得简洁

struct ListNode* addTwoNumbers(struct ListNode* l1, struct ListNode* l2) {
    struct ListNode* head;
    struct ListNode* temp;
    struct ListNode* s;
    struct ListNode* p;

    head=(struct ListNode*)malloc(sizeof(struct ListNode));
    head->next=NULL;

    int carry=0;
    int sum;

    p=head;
    while (l1||l2||carry) {
        sum=(l1?l1->val:0) + (l2?l2->val:0) + carry;

        s=(struct ListNode*)malloc(sizeof(struct ListNode));
        s->val=sum % 10;
        carry=sum / 10;
        s->next=NULL;

        p->next=s;
        p=p->next;

        l1=(l1?l1->next:l1);
        l2=(l2?l2->next:l2);
    }

    temp=head;
    head=head->next;
    free(temp);
    return head;
}
```