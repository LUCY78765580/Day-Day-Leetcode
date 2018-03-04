- **堆栈**：具有一定操作约束的线性表，只能在一端作插入、删除
- 具有**后入先出**的特性（Last In First Out）
- 分**顺序存储**结构、**链式存储**结构两种形式
<br>
<br>


## 栈的顺序存储结构
通常由一个**一维数组**和一个**栈顶元素变量**组成

图解如下：

![](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/stack001.jpg)

![](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/stack002.jpg)

![](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/stack003.jpg)

<br>
<br>

### 0、结构初始化
```c
#define MaxSize ###
struct StackNode {
    ElementType Data[MaxSize];
    int top;
};
```
<br>
<br>

### 1、建立空栈
```c
struct StackNode* makeEmpty() {
    struct StackNode* s=malloc(sizeof(struct StackNode));
    s->top=-1;
    return s;
}
```
<br>
<br>

### 2、push操作
```c
void push(struct StackNode* s,ElementType x) {
    if (s->top==MaxSize-1)
        return ;
    else {
        s->Data[++(s->top)]=x;
        return ;
    }
}
```
<br>
<br>

### 3、pop操作
```c
ElementType pop(struct StackNode* s) {
    if (s->top==-1)
        return ERROR;
    else {
        return s->Data[(s->top)--];
    }
}
```
