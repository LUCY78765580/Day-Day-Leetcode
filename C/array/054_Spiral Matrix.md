## 054  Spiral Matrix
[题目地址](https://leetcode.com/problems/spiral-matrix/description/)
<br>
<br>
<br>

```c
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */

//螺旋数组
//题目有点绕，绕的有点晕

int* spiralOrder(int** matrix, int matrixRowSize, int matrixColSize) {
    int totalSize=matrixRowSize*matrixColSize;
    int* res=(int*)malloc(sizeof(int)*totalSize);

    int index=0;
    int u=0;
    int d=matrixRowSize-1;
    int l=0;
    int r=matrixColSize-1;

    while (true) {
        //up,to right
        for (int col=l;col<=r;col++) res[index++]=matrix[u][col];
        if (++u>d) break;

        //right,to down
        for (int row=u;row<=d;row++) res[index++]=matrix[row][r];
        if (--r<l) break;

        //down,to left
        for (int col=r;col>=l;col--) res[index++]=matrix[d][col];
        if (--d<u) break;

        //left,ro up
        for (int row=d;row>=u;row--) res[index++]=matrix[row][l];
        if (++l>r) break;

    }
    return res;
}
```