## 队列的链式存储结构（不常用）
<br>

- 同理，实际上也可以用一个**单链表**实现
- 插入、删除分别在链表**两头**进行,即**插入在表尾（rear），删除在表头(front)**

图解如下：

![](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/deque002.jpg)


### 0、结构初始化
```c
struct ListNode {
    ElementType val;
    struct ListNode* next;
};
struct QueueNode {
    int size;
    struct ListNode* front;
    struct ListNode* rear;
};
```
<br>

### 1、建立空队列 createQueue
```c
struct QueueNode* createQueue() {
    struct QueueNode* q=malloc(sizeof(struct QueueNode));
    q->front = q->rear = NULL;
    q->size=0;

    return q;
}
```
<br>

### 2、入队操作addQueue
//插入在表尾
```c
void addQueue(struct QueueNode* q,ElementType x) {
    struct ListNode* temp=malloc(sizeof(struct ListNode));
    temp->val=x;

    if (q->size==0) {
        q->front = q->rear =temp;
    }
    else {
        q->rear->next=temp;
        q->rear=temp;
    }

    q->size++;
}
```
<br>

### 3、出队操作deleteQueue
//删除在表头
```c
ElementType deleteQueue(struct QueueNode* q) {
    struct ListNode* temp;
    ElementType tmp;

    if (q->size==0) return ERROR;
    else {
        temp=q->front;
        q->front=temp->next;
        tmp=temp->val;

        free(temp)
        q->size--;
        return tmp;
    }
}
```
