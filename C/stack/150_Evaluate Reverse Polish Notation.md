## 150  Evaluate Reverse Polish Notation
[题目地址](https://leetcode.com/problems/evaluate-reverse-polish-notation/description/)
<br>
<br>
<br>

```c
//逆波兰表达式求值

//逆波兰表达式，又叫后缀表达式
//本题关键：1、弄清逆波兰表达式求值规律 2、定义一个函数将字符串转换为整数

//堆栈形式1
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

void push(struct StackNode* s,int x) {
    struct ListNode* temp=malloc(sizeof(struct ListNode));
    temp->val=x;

    if (s->size==0) s->top=temp;
    else {
        temp->next=s->top;
        s->top=temp;
    }

    s->size++;
}

int pop(struct StackNode* s) {
    struct ListNode* temp;
    int tmp;

    if (s->size==0) return NULL;
    else {
        temp=s->top;
        tmp=temp->val;
        s->top=temp->next;

        free(temp);

        s->size--;
        return tmp;
    }
}

int peek(struct StackNode* s) {
    if (s&&s->top) {
        return s->top->val;
    }
    return NULL;
}


//堆栈形式2
/*
struct StackNode {
    int val;
    struct StackNode* next;
};

//构造堆栈,s为头结点，插入删除都在表头进行
struct StackNode* createStack() {
    struct StackNode* s=malloc(sizeof(struct StackNode));
    s->next=NULL;

    return s;
}

void push(struct StackNode* s,int x) {
    struct StackNode* temp=malloc(sizeof(struct StackNode));
    temp->val=x;
    temp->next=s->next;
    s->next=temp;
}

int pop(struct StackNode* s) {
    struct StackNode* temp;
    int tmp;

    if (s->next!=NULL) {
        temp=s->next;
        tmp=temp->val;
        s->next=temp->next;

        free(temp);
        return tmp;
    }
    else
        return NULL;
}

int peek(struct StackNode* s) {
    if (s->next!=NULL) {
        return s->next->val;
    }
    return NULL;
}
*/

//堆栈形式3
/*
#define MaxSize 1000000
struct StackNode {
    int Data[MaxSize];
    int top;
};

struct StackNode* createStack() {
    struct StackNode* s=malloc(sizeof(struct StackNode));
    s->top=-1;

    return s;
}

void push(struct StackNode* s,int x) {
    if (s->top!=MaxSize-1)
        s->Data[++(s->top)]=x;
    else
        return NULL;
}

int pop(struct StackNode* s) {
    if (s->top!=-1)
        return s->Data[(s->top)--];
    else
        return NULL;
}

int peek(struct StackNode* s) {
    if (s->top!=-1)
        return s->Data[s->top];
    else
        return NULL;
}
*/


//将字符串中数字转换为整型
//注意这里仅仅对于含有数字的字符串
int ati(char* s) {
    int n=0;
    int sign=0;

    while (*s!='\0') {
        switch(*s) {
            case '+':
                break;
            case '-':
                sign=-1;
            case '*':
                break;
            case '/':
                break;
            default:
                n=n*10 + (s[0]-'0');
                //注意这里是'0'而不是'\0'
        }
        s++;
    }

    return sign?-n:n;
}

int evalRPN(char** tokens,int tokensSize) {
    int i;
    int m,n;
    int c;

    struct StackNode* stack=createStack();

    for (i=0;i<tokensSize;i++) {
        if (tokens[i][0]=='\0')
            break;
        else if (tokens[i][0]=='+') {
            push(stack,(pop(stack)+pop(stack)));
        }
        else if (tokens[i][0]=='-'&&tokens[i][1]=='\0') {
            n=pop(stack);
            m=pop(stack);
            push(stack,(m-n));
        }
        else if (tokens[i][0]=='*') {
            push(stack,(pop(stack)*pop(stack)));
        }
        else if (tokens[i][0]=='/') {
            n=pop(stack);
            m=pop(stack);
            if (n!=0)
                push(stack,m/n);
            else
                return 0;
        }
        else {
            //token[i]得到所指的字符串
            //注意这里的顺序，上面的tokens[i][0]=='-'&& tokens[i][1]='\0'
            //已经过滤掉所有的"-",剩下的就只有"-数字"形式，配合ati函数不会产生冲突
            c=ati(tokens[i]);
            push(stack,c);
        }

    }

    return peek(stack);
}
```