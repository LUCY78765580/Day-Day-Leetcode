## 160 Intersection of Two Linked Lists
[题目地址](https://leetcode.com/problems/intersection-of-two-linked-lists/description/)
<br>
<br>
<br>

图解如下：

![](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/linear-list007.jpg)

![](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/linear-list008.jpg)

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

//链表相交
//审题：链表中无环

struct ListNode *getIntersectionNode(struct ListNode *headA, struct ListNode *headB) {
    struct ListNode* a=headA;
    struct ListNode* b=headB;
    int i=0;

    //边界条件，headA/headB不存在
    if (!a||!b)
        return NULL;

    //a不等于b时，展开下面
    //如果a==b,直接最后return a
    while (a!=b){
        a=a->next;
        b=b->next;

        //a、b走到头后，指针拨向对方，计数器i加1
        //规律：如果有交点，接下来a,b将走过相同步数
        //break出循环，不可能有第二次i++，所以i>=2,表示没有交点
        if (a==NULL){
            a=headB;
            i++;
        }
        if (b==NULL){
            b=headA;
        }
        if (i==2)
            return NULL;
    }

    return a;
}
```

