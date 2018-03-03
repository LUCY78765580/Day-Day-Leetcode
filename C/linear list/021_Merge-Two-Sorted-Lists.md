## 021 Merge Two Sorted Lists
[题目地址](https://leetcode.com/problems/merge-two-sorted-lists/description/)
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

//方法一：一般的插入方法
//主要思想，遍历l1中数据，并往l2中插入(最后返回l2)
//分插入表头/表中，两种情况
struct ListNode* mergeTwoLists(struct ListNode* l1, struct ListNode* l2) {
    struct ListNode* p;
    struct ListNode* q;

    if (l1==NULL) return l2;
    if (l2==NULL) return l1;

    //遍历l1中数据，首先脱离出一个p出来
    while (l1) {
        p=l1;l1=l1->next;

        //1）在表头插入
        if (p->val <= l2->val) {
            p->next=l2;
            l2=p;
        }

        //2）在表中插入
        else {
            q=l2;
            while(q&&q->next) {
                //如果p的值恰好小于等于q->next(已经大于q),直接插入并break
                //如果p的值大于q->next,q往下走
                if (p->val <= q->next->val){
                    p->next=q->next;
                    q->next=p;
                    break;
                }
                else
                    q=q->next;
            }
            //如果q->next为NULL，说明q是l2中最后一个数据了
            //这时直接将p接入l2最后即可
            if (q->next==NULL) {
                q->next=p;
                p->next=NULL;
            }
        }
    }

    //最后返回l2
    return l2;
}


//方法二：使用递归(操作简单，理解稍难)
struct ListNode* mergeTwoLists(struct ListNode* l1,struct ListNode* l2) {
    struct ListNode* p;
    struct ListNode* q;

    if (l1==NULL) return l2;
    if (l2==NULL) return l1;

    if (l1->val <= l2->val) {
        l1->next=mergeTwoLists(l1->next,l2);
        return l1;
    }
    else {
        l2->next=mergeTwoLists(l1,l2->next);
        return l2;
    }
}
```