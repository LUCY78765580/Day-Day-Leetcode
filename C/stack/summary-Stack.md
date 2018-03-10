- **堆栈**：具有一定操作约束的线性表，只能在一端作插入、删除
- 具有**后入先出**的特性（Last In First Out）
- 分**顺序存储**结构、**链式存储**结构两种形式
<br>
<br>


## 堆栈的顺序存储结构
通常由一个**一维数组**和一个**栈顶元素变量**组成

图解如下：

![](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/stack001.jpg)

![](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/stack002.jpg)

![](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/stack003.jpg)

<br>
<br>
<br>

## 形式一：构建结构体
### 0、结构初始化
```c
#define MaxSize ###
struct StackNode {
    ElementType Data[MaxSize];
    int top;
};
```
<br>

### 1、建立空栈
```c
struct StackNode* createStack() {
    struct StackNode* s=malloc(sizeof(struct StackNode));
    s->top=-1;

    return s;
}
```
<br>

### 2、push操作
```c
void push(struct StackNode* s,ElementType x) {
    if (s->top!=MaxSize-1)
        return s->Data[++(s->top)]=x;
    else
        return NULL;
}
```
<br>

### 3、pop操作
```c
ElementType pop(struct StackNode* s) {
    if (s->top!=-1)
        return s->Data[(s->top)--];
    else
        return NULL;
}
```

### 4、peek操作
```c
ElementType peek(struct StackNode* s) {
    if (s->top!=-1)
        return s->Data[s->top];
    else
        return NULL;
}
```
<br>
<br>
<br>

## 形式二：简易模式
也可以省略结构体部分，直接声明数组，在函数中构建堆栈
<br>

```c
//举例020 Valid Parentheses 这一题
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

