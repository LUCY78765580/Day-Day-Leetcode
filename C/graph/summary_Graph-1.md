## 邻接矩阵表示图
<br>
<br>
<br>

![](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/graph001.jpg)
<br>
![](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/graph002.jpg)
<br>
![](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/graph003.jpg)
<br>
![](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/graph004.jpg)
<br>
![](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/graph005.jpg)
<br>
![](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/graph006.jpg)
<br>
<br>
<br>

### 0、结构初始化
```c
struct GraphNode {
    int Nv; /* 顶点数 */
    int Ne; /* 边数   */
    WeightType G[maxVertexNum][maxVertexNum];
    ElementType data[maxVertexNum]; /* 存顶点的数据 */
};
struct EdgeNode {
    int v1,v2; /* 有向边 */
    WeightType weight; /* 权重 */
};
```
<br>


### 1、图的初始化
```c
//初始化一个有vertexNum个顶点，但是没有边的图
struct GraphNode* createGraph(int vertexNum) {
    struct GraphNode* Graph;
    Graph=(struct GraphNode*)malloc(sizeof(struct GraphNode));
    Graph->Nv=vertexNum;
    Graph->Ne=0;

    /* 默认编号从顶点0开始 */
    for (int i=0;i<Graph->Nv;i++) {
        for (int j=0;j<Graph->Nv;j++) {
            Graph->G[i][j]=0;
        }
    }

    return graph;
}
```
<br>


### 2、向图中插入边
```c
void insertEdge(struct GraphNode* graph,struct EdgeNode* E) {
    Graph->G[E->v1][E->v2]=E->weight;
    /* 如果是无向图,还要插入边<v2,v2> */
    Graph->G[E->v2][E->v1]=E->weight;
}
```
<br>


### 3、完整建立一个Graph-1
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
        scanf("%c",&(Graph->data[j]));
    }

    return Graph;
}
```


### 4、完整建立一个Graph-2(简化版本)
```c
int G[maxNum][maxNum],Nv,Ne;
void buildGraph() {
    int v1,v2,weight;

    scanf("%d",Nv);
    for (int i=0;i<Graph->Nv;i++) {
        for (int j=0;j<Graph->Nv;j++) {
            Graph->G[i][j]=0;
        }
    }
    scanf("%d",Ne);
    for (int k=0;k<Ne;k++) {
        scanf("%d %d %d",&v1,&v2,&weight);
    }

    G[v1][v2]=weight;
    G[v2][v2]=weight;
}
```
<br>
<br>
