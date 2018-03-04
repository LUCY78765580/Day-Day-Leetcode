**线性表**：由**同类**数据元素构成的**有序**序列的**线性**结构

实现方式分两种:**顺序存储实现**（ArrayList）和**链式存储实现**（LinkList）
<br>

## 顺序表

### 0、结构初始化
顺序表结构如图：

![](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/linear-list001.jpg)

```c
struct ListNode {
    ElementType Data[MaxSize];  //定义一个大小为MaxSize的数组
    int Last;                   //最后一个元素所在的位置
};
```
<br>

### 1、建立（空顺序表）
```c
struct ListNode* MakeEmpty() {
    struct ListNode* P;

    P=(struct ListNode*)malloc(sizeof(struct ListNode));
    P->Last=-1;
    return p;
}
```
<br>

### 2、查找
```c
//找不到则返回-1，找到则返回i
int Find(ElementType x,struct ListNode* P) {
    int i=0;

    while (i<=P->Last&&P->Data[i]!=x) i++;
    if (i>P->Last)
        return -1;
    else
        return i;
}
```
<br>

### 3、插入
图解如下:

![](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/linear-list002.jpg)

```c
//第 i (1≤i≤n+1)个位置上插入一个值为X的新元素
//即在下标为i-1位置上，插入元素x
void Insert(ElementType x,int i,struct ListNode* P) {
    int j;

    //判断表空间是否已满
    if (p->Last==MaxSize-1) return ;
    //检查插入合法性
    if (i<1 || i>p->Last+2) return ;

    //数据往后挪，然后再插入
    for (j=p->Last;j>=i-1;j--) {
        p->Data[j+1]=p->Data[j];
    }
    p->Data[i-1]=x;
    p->Last++;
}
```
<br>

### 4、删除
图解如下：

![](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/linear-list003.jpg)

```c
//删除表的第 i (1≤i≤n)个位置上的元素
//即删除下标为i-1位置的元素
void Delete(int i,struct ListNode* p) {
    int j;

    //判断元素是否存在
    if (i<1 || i>p->Last+1) return ;

    for (j=i-1;j<p->Last;j++) {
        p->Data[j]=p->Data[j+1];
    }
    p->Last--;
}
```

