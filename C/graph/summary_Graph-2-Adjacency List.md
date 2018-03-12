## 邻接表表示图
<br>
<br>
<br>

![](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/graph007.jpg)
<br>
![](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/graph008.jpg)
<br>
<br>
<br>

对于邻接表，G[N]为**指针数组**，对应矩阵**每行一个链表**，只存非0元素
- 指针数组里的每一个指针都是一个**单链表的头指针**，单链表里每个**节点**里存储的是图中**每条边**的信息。
- 邻接表包括一个**顶点表**和一个**边表**。顶点表包括顶点和指向下一个邻接点的指针，
边表存储的是邻接点点序号和指向下一个的指针刚开始的时候把顶点表初始化，指针指向null。
然后边表插入进来，是插入到前一个，也就是直接插入到firstedge指向的下一个，而后面的后移
<br>

### 0、结构初始化
```c
//对邻接点（弧节点/边表节点）
struct ENode {
    int position;      /* 邻接点下标 */
    WeightType weight;    /* 边权重 */
    EdgeNode* next;    /* next指针 */
};

//对于头结点（顶点表节点）
typedeft struct VNode {
    struct ENode* firstEdge; /* 第一个表结点的地址,指向第一条依附该顶点的弧的指针 */
    ElementType data;           /* 存顶点数据 */
}AdjList[maxVertexNum];

//对整个图
struct GraphNode {
    int Nv; /* 顶点数 */
    int Ne; /* 边数   */
    AdjList G; /* 邻接表(数组),AdjList为邻接表类型 */
};

//边结构
struct EdgeNode {
    int v1,v2; /* 有向边 */
    WeightType weight; /* 权重 */
};
```
<br>


### 1、图的初始化
```c
//初始化一个有VertexNum个顶点但没有边的图
struct GraphNode* createGraph(int vertexNum) {
    struct GraphNode* Graph;
    Graph=(struct GraphNod*)malloc(sizeof(struct GraphNode));
    Graph->Nv=vertexNum;
    Graph->Ne=0;

    /* 注意顶点编号从0开始 */
    for (int i=0;i<Graph->Nv;i++) {
        Graph->G[i].firstEdge=NULL;
    }

    return Graph;
}
```
<br>


### 2、向图中插入边
图解如下：

![](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/graph009.jpg)
<br>

```c
void insertEdge(struct GraphNode* Graph,struct EdgeNode* E) {
    struct ENode* temp;

    /* 将边<v1,v2>插入,此时已经有v1在表头了 */
    /* 为v2创建新的邻接点 */
    /* 将v2插入v1的表头 */
    temp=(struct ENode*)malloc(sizeof(struct ENode));
    temp->position=E->v2;
    temp->weight=E->weight;
    temp->next=Graph->G[v1].firstEdge;
    Graph->G[v1].firstEdge=temp;

    /* 如果是无向图,还要插入边<v2,v1> */
    /* 为v1创建新的邻接点 */
    /* 将v1插入v2的表头 */
    temp=(struct ENode*)malloc(sizeof(struct ENode));
    temp->position=E->v1;
    temp->weight=E->weight;
    temp->next=Graph->G[v2].firstEdge;
    Graph->G[v2].firstEdge=temp;
}
```
<br>


### 3、完整建立一个Graph
//与前面邻接矩阵基本相同，只有小小的差别
```c
struct GraphNode* buildGraph() {
    struct GraphNode* Graph;
    struct EdgeNode* E;
    int Nv;

    scanf("%d",&Nv);
    Graph=createGraph(Nv);
    scanf("%d",&Ne);
    if (Graph->Ne!=0) {
        E=(struct EdgeNode*)malloc(sizeof(struct EdgeNode));
        for (int i=0;i<Ne;i++) {
            scanf("%d %d %d",E->v1,E->v2,E->weight);
            insertEdge(Graph,E);
        }
    }
    /* 如果顶点有数据的话存入数据 */
    for (int j=0;j<Nv;j++) {
        scanf("%c",&(Graph->G[i].Data));
        /* 仅仅是这里与前面不一样 */
    }

    return Graph;
}
```



