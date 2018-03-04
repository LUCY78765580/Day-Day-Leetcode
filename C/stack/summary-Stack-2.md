## 栈的链式存储结构—链栈
<br>

- 实际上是一个**单链表**
- 插入、删除**只能在栈的栈顶**进行（**栈顶指针不能在链尾！！**）

图解如下：

![](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/stack004.jpg)

<br>
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
<br>

### 1、建立空栈
```c
struct CreateStack() {
    struct StackNode* s=malloc(sizeof(struct StackNode));
    s->size=0;
    s->top=NULL;
    return s;
}
```
<br>
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
<br>

### 3、pop操作
```c
void pop(struct StackNode* s) {
    struct ListNode* temp=s->top;
    s->top=temp->next;
    free(temp);

    s->size--;
}
```
<br>
<br>

### 4、取栈顶元素
```c
ElementType top(struct StackNode* s) {
    if (s&&s->top) {
        return s->top->val;
    }
    return 0;
}
```
<br>
<br>

### 5、destroy操作
```c
void destroyStack(struct StackNode* s) {
    while (s->size) {
        pop(s);
    }
    free(s);
}
```


