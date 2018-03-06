## 链表
链表相对于顺序表，不需要移动数据元素，只需要修改“链”，所以在某些场合要显得更灵活
<br>
<br>
<br>

### 0、结构初始化
图解如下：

![](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/linear-list004.jpg)

```c
struct ListNode {
    ElementType val;
    struct ListNode* next;
};
```
<br>

### 1、建立(空链表)
```c
struct ListNode* CreateEmpty() {
    struct ListNode* p;
    p=(struct ListNode*)malloc(sizeof(ListNode));
    p->next=NUll;
}
```
<br>

### 2、求表长
```c
int Length(struct ListNode* p) {
    int j=0;

    while (p) {
        p=p->next;
        j++;
    }
    return j;
}
```
<br>

### 3、查找
```c
//按照值查找
struct ListNode* Find(ElementType x,struct ListNode* p) {
    while (p&&p->val!=x)
        p=p->next;
    return p;
}
```

```c
//按照序号查找
//查找第k个元素,k从1开始
struct ListNode* FindKth(int k,struct ListNode* p) {
    int i=1;
    while (p!=NULL&&i<k) {
        p=p->next;
        i++;
    }

    if (i==k) return p;
    else return NULL;
}
```
<br>

### 4、插入（在第i-1节点后面插入）
图解如下：

![](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/linear-list005.jpg)

```c
//插入位置分两种情况：在表头/不在表头
/*
1）先构造一个新节点s
2）找到链表第i-1个节点q
3）x->next=p->next;p->next=s;(不可颠倒)
*/

struct ListNode* Insert(struct ListNode* p,int i,ElementType x) {
    struct ListNode* q,s;

    if (i==1) {
        s=(struct ListNode*)malloc(sizeof(struct ListNode));
        s->val=x;
        s->next=p;
        return s;
    }

    q=FindKth(i-1,p);
    if (q==NULL)
        return NULL;
    else {
        s=(struct ListNode*)malloc(sizeof(struct ListNode));
        s->val=x;
        s->next=q->next;
        q->next=s;
        return p;
    }
}
```
<br>

### 5、删除（删除链表第i个节点）
图解如下：

![](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/linear-list006.jpg)

```c
//同理删除位置分两种：在表头/非表头
/*
1)首先找到第i-1个节点q
2)指针s指向待删除节点 s=q->next;
3)修改指针删除s节点   q->next=s->next;
4）释放s节点   free(s);
*/

struct ListNode* Delete(struct ListNode* p,int i) {
    struct ListNode* q,s;

    if (p==NULL) return NULL;
    if (i==1) {
        s=p;
        p=p->next;
        free(s);

        return p;
    }

    q=FindKth(i-1,p);
    if (q==NULL) return NULL;
    else if (q->next==NULL) return NULL;
    else {
        s=q->next;
        q->next=s->next;
        free(s);

        return p;
    }
}
```

