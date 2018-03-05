## 栈的链式存储结构—链栈
<br>

- 实际上是一个**单链表**
- 插入、删除**只能在栈的栈顶**进行（**栈顶指针不能在链尾！！**）

图解如下：

![](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/stack004.jpg)

<br>

## 形式一
构造ListNode和StackNode
<br>

### 0、结构初始化
```c
struct ListNode {
    ElementType val;
    struct ListNode* next;
};
struct StackNode {
    int size;
    struct ListNode* top;
};
```
<br>

### 1、建立空栈
```c
struct StackNode* CreateStack() {
    struct StackNode* s=malloc(sizeof(struct StackNode));
    s->size=0;
    s->top=NULL;
    return s;
}
```
<br>

### 2、push操作
```c
void push(struct StackNode* s,ElementType x) {
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
```
<br>

### 3、pop操作
```c
ElementType pop(struct StackNode* s) {
    struct ListNode* temp;
    ElementType tmp;

    if (s->size==0) return NULL;
    else {
        temp=s->top;
        s->top=temp->next;
        tmp=s->top->val;
        free(temp);

        s->size--;
        return tmp;
    }
}
```
<br>

### 4、取栈顶元素
```c
ElementType peek(struct StackNode* s) {
    if (s&&s->top) {
        return s->top->val;
    }
    return NULL;
}
```
<br>
<br>
<br>

## 形式二
仅仅构造StackNode(StackNode此时即为一个链表)
<br>

### 0、结构初始化
```c
struct StackNode {
    ElementType val;
    struct ListNode* next;
};
```
<br>

### 1、建立空栈
```c
struct StackNode* CreateStack() {
    struct StackNode* s=malloc(sizeof(struct StackNode));
    s->next=NULL

    return s;
}
```
<br>

### 2、push操作
```c
void push(struct StackNode* s,ElementType x) {
    struct StackNode* temp=malloc(sizeof(struct StackNode));
    temp->val=x;
    temp->next=s->next;
    s->next=temp;
}
```
<br>

### 3、pop操作
```c
ElementType pop(struct StackNode* s) {
    struct StackNode* temp;
    ElementType tmp;

    if (s->next!=NULL) {
        temp=s->next;
        s->next=temp->next;
        tmp=temp->val;

        free(temp);
        return tmp;
    }
    else
        return NULL;
}
```
<br>

### 4、取栈顶元素
```c
ElementType peek(struct StackNode* s) {
    if (s->next!=NULL) {
        return s->next->val;
    }
    else
        return NULL;
}
```



