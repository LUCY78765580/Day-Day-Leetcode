## 227 Mock Hanoi Tower by Stacks
[题目地址](http://www.lintcode.com/zh-cn/problem/mock-hanoi-tower-by-stacks/)

这一题是Lintcode 的题
<br>
<br>

**汉诺塔**：又称为河内塔问题。有位僧人整天把三根柱(x,y,z或者a,b,c)上的金盘倒来倒去，原来他是想把64个一个比一个小的金盘从一根柱子上移到另一根柱子上去。

移动过程中遵守以下规则：

(1) 每次只能移动一个盘子。

(2) 每个盘子从堆的顶部被移动后，只能置放于下一个堆中。

(3) 每个盘子只能放在比它大的盘子上面。
<br>
图解如下：

![](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/stack006.jpg)

<br>
<br>

```c
//可以先建立栈结构
//绕后a中n-1个倒入b中，最后1个倒入c中，再从b中将这n-1个倒入c中
//建立栈
struct StackNode {
    int val;
    struct ListNode* next;
};

struct StackNode* CreateStack() {
    struct StackNode* s=malloc(sizeof(struct StackNode));
    s->next=NULL

    return s;
}

bool isEmpty(struct StackNode* s) {
    return s->next==NULL;
}

void push(struct StackNode* s,ElementType x) {
    struct StackNode* temp=malloc(sizeof(struct StackNode));
    temp->val=x;
    temp->next=s->next;
    s->next=temp;
}

int pop(struct StackNode* s) {
    struct StackNode* temp;
    int tmp;

    if (!isEmpty(s)) {
        temp=s->next;
        s->next=temp->next;
        tmp=temp->val;

        free(temp);
        return tmp;
    }
    else
        return NULL;
}

int peek(struct StackNode* s) {
    if (!isEmpty(s)) {
        return s->next->val;
    }
    else
        return NULL;
}

//pop_push函数
void pop_push(struct StackNode* s1,struct StackNode* s2) {
    int x;
    if (!isEmpty(s1) {
        x=pop(s1);
        push(s2,x);
    }
}

//总函数
void hanoi(struct StackNode* a,struct StackNode* b,struct StackNode* c,int numsSize) {
    if (numsSize == 1) {
        pop_push(a,c);
    }
    else {
        hanoi(a,c,b,n-1); //将栈sa 前面n-1 个盘子顺序移到栈sb
        pop_push(a,c);    //将栈sa 第n个 盘子移到栈sc
        hanoi(b,c,a,n-1); //将栈sb n-1 个盘子顺序移到栈sc
    }
}
```