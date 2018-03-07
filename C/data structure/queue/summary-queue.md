- **队列**：具有一定操作约束的线性表，只能在一端作插入、删除，与堆栈类似
- 具有**先入先出**的特性（First In First Out）
- 同理，分**顺序存储**结构、**链式存储**结构两种形式
<br>
<br>


## 队列（顺序存储结构）
- 通常由一个**一维数组**和一个**队列头元素变量front**和一个**队列尾元素变量rear**组成
- 加入一个元素rear加1，删除一个元素front加1
- 空的时候front=rear,但是填满时front/rear也相等，这时便不利于区分；为此通常采用**“加1求余”**的方式,同时构成**循环队列**

- 1）判断是否为空：front == rear 即为空
- 2）判断是否为满：（rear+1）%MaxSize == front 即为满

图解如下：

![](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/deque000.jpg)

![](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/deque001.jpg)
<br>

### 0、结构初始化
```c
#define Size ###
struct QueueNode {
    ElementType *Data;
    int MaxSize;
    int front;
    int rear;
};
```
<br>

### 1、建立空队列 createQueue
```c
struct QueueNode* createQueue() {
    struct QueueNode* q=malloc(sizeof(struct QueueNode));
    q->Data=(ElementType*)malloc(MaxSize*sizeof(ElementType));
    q->front=q->rear=0;
    q->MaxSize=Size;

    return q;
}
```
<br>

### 2、判断队列是否充满 isFull
```c
bool isFull(struct QueueNode* q) {
    return ( (q->rear+1)%q->MaxSize == q->front );
}
```
<br>

### 3、判断队列是否为空 isEmpty
```c
bool isEmpty(struct QueueNode* q) {
    return ( q->rear == q->front );
}
```
<br>

### 4、入队操作addQueue
```c
void addQueue(struct QueueNode* q,ElementType x) {
    if (isFull(q)) return false;
    else {
        q->rear = (q->rear+1)%q->MaxSize;
        q->Data[q->rear]=x;
    }
}
```
<br>

### 5、出队操作deleteQueue
```c
ElementType deleteQueue(struct QueueNode* q) {
    if (isEmpty(q)) return false;
    else {
        q->front = (q->front+1)%q->MaxSize;
        return q->Data[q->front];
    }
}
```
