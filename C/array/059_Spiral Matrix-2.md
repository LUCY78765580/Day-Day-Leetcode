## 052  Spiral Matrix II
[题目地址](https://leetcode.com/problems/spiral-matrix-ii/description/)
<br>
<br>
<br>

```c
/**
 * Return an array of arrays.
 * Note: The returned array must be malloced, assume caller calls free().
 */

//和上一题相差无几
//注意要malloc的
int** generateMatrix(int n) {
    int totalSize=n*n;
    int** res=(int**)malloc(sizeof(int*)*totalSize);
    for (int t=0;t<n;t++) {
        res[t]=(int*)malloc(sizeof(int)*n);
    }

    int i=1;
    int u=0;
    int d=n-1;
    int l=0;
    int r=n-1;


    while (true) {
        //up,to right
        for (int col=l;col<=r;col++) res[u][col]=i++;
        if (++u>d) break;

        //right,to down
        for (int row=u;row<=d;row++) res[row][r]=i++;
        if (--r<l) break;

        //down,to left
        for (int col=r;col>=l;col--) res[d][col]=i++;
        if (--d<u) break;

        //left,to up
        for (int row=d;row>=u;row--) res[row][l]=i++;
        if (++l>r) break;
    }
    return res;
}
```