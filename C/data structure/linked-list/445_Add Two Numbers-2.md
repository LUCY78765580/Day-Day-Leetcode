## 445  Add Two Numbers II
[题目地址](https://leetcode.com/problems/add-two-numbers-ii/description/)
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

//与之前类似，但是从后面相加
//但是直接相加比较麻烦


//方法一：使用堆栈(非常方便，容易理解)
struct StackNode {
    int size;
    struct ListNode* top;
};

struct StackNode* createStack() {
    struct StackNode* s=malloc(sizeof(struct StackNode));
    s->size=0;
    s->top=NULL;

    return s;
}

void push(struct StackNode* s,int x) {
    struct ListNode* temp=malloc(sizeof(struct ListNode));
    temp->val=x;
    temp->next=NULL;

    if (s->size==0) s->top=temp;
    else {
        temp->next=s->top;
        s->top=temp;
    }

    s->size++;
}

void pop(struct StackNode* s) {
    struct ListNode* temp=s->top;
    s->top=temp->next;
    free(temp);

    s->size--;
}

int top (struct StackNode* s) {
    if (s&&s->top)
        return s->top->val;
    return 0;
}

void destroyStack(struct StackNode* s) {
    while (s->size) {
        pop(s);
    }
    free(s);
}

void traverse(struct ListNode* p,struct StackNode* s) {
    while (p) {
        push(s,p->val);
        p=p->next;
    }
}


//主函数
struct ListNode* addTwoNumbers(struct ListNode* l1,struct ListNode* l2) {
    struct StackNode* s1=createStack();
    struct StackNode* s2=createStack();
    struct StackNode* ans=createStack();

    traverse(l1,s1);
    traverse(l2,s2);
    int sum=0;

    while (s1->size || s2->size) {
        if (s1->size) {
            sum+=top(s1);
            pop(s1);
        }
        if (s2->size) {
            sum+=top(s2);
            pop(s2);
        }
        push(ans,sum%10);
        sum /= 10;
    }

    //注意这一步，如果最后sum不为0，仍然要加到最后一位
    if (sum) push(ans,sum);
    destroyStack(s1);
    destroyStack(s2);

    return ans->top;
}


//方法二：使用递归
//难，暂且先放着
//[参考答案](https://leetcode.com/problems/add-two-numbers-ii/discuss/92800/C-recursive-solution-(22-ms)-no-original-list-modreversal)
```
