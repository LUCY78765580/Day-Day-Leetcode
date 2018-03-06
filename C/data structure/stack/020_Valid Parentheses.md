## 020 Valid Parentheses
[题目地址](https://leetcode.com/problems/valid-parentheses/description/)
<br>
<br>
<br>

```c
/*原题中已经暗含
struct ListNode {
    char val;
    struct ListNode* next;
};
*/

//方法一：堆栈（基于链表 ）
struct StackNode {
    int size;
    struct ListNode* top;
};

struct StackNode* createStack() {
    struct StackNode* s=malloc(sizeof(struct StackNode));
    s->top=NULL;
    s->size=0;

    return s;
}

bool isEmpty(struct StackNode* s) {
    return (s->size==0);
}

void push(struct StackNode* s,char x) {
    struct ListNode* temp=malloc(sizeof(struct ListNode));
    temp->val=x;

    if (s->size==0) s->top=temp;
    else {
        temp->next=s->top;
        s->top=temp;
    }

    s->size++;
}

char pop(struct StackNode* s) {
    struct ListNode* temp;
    char tmp;

    if (s->size==0) return NULL;
    else {
        temp=s->top;
        s->top=temp->next;
        tmp=temp->val;
        free(temp);

        s->size--;
        return tmp;
    }
}

char peek(struct StackNode* s) {
    if (s&&s->top) {
        return s->top->val;
    }
    return NULL;
}

bool isValid(char* s) {
    struct StackNode* stack=createStack();
    int len=strlen(s);

    //如果s为空，返回false
    if (*s==NULL) return false;

    //如果为奇数个，返回false
    if (len%2==1) return false;

    //如果为偶数个，用while循环判断
    while (*s!='\0') {
        char c=*s;
        if (c=='('||c=='{'||c=='[') {
            push(stack,c);
        }
        else {
            if (c==')'&&!isEmpty(stack)&&peek(stack)=='(')
                pop(stack);
            else if (c==']'&&!isEmpty(stack)&&peek(stack)=='[')
                pop(stack);
            else if (c=='}'&&!isEmpty(stack)&&peek(stack)=='{')
                pop(stack);
        }

        s++;
    }

    //最后栈一定是空的，否则证明不能完全匹配
    return isEmpty(stack);
}



//方法二：堆栈（基于数组）
bool isValid(char* s) {
    int len=strlen(s);
    if (*s==NULL) return false;
    if (len%2==1) return false;

    char stack[1000000];
    int top=-1;

    while (*s) {
        char c=*s;
        if (c=='('||c=='{'||c=='[') {
            stack[++top]=c;
        }
        else {
            if (c==')'&&top>=0&&stack[top]=='(')
                top--;
            else if (c==']'&&top>=0&&stack[top]=='[')
                top--;
            else if (c=='}'&&top>=0&&stack[top]=='{')
                top--;
        }
        s++;
    }

    return top==-1;
}
```
