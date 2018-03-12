## 073  Set Matrix Zeroes
[题目地址](https://leetcode.com/problems/set-matrix-zeroes/description/)
<br>
<br>
<br>

```c
//题目意思：如果某行或某列为0，那么对应十字上都为0
//不能理解成：如果矩阵中有一个数为0,那么所有行和所有列都为0

//首先由matrix[i][j]=0,标记matrix[i][0](第一列)为0
//由matrix[i][0]位置为0，标记col0为0

//由col0为0,使得整一列matrix[i][0]为0
//在第二个循环中，使得matrix[i][j]为0，进而遍布所有范围

void setZeroes(int** matrix, int matrixRowSize, int matrixColSize) {
    int rows=matrixRowSize;
    int cols=matrixColSize;
    int flag=1;

    //第一次循环
    for (int i=0;i<rows;i++) {
        if (matrix[i][0]==0) flag=0;
        for (int j=1;j<cols;j++) {
            if (matrix[i][j]==0) {
                matrix[i][0]=matrix[0][j]=0;
            }
        }
    }

    //第二次循环
    for (int i=rows-1;i>=0;i--) {
        for (int j=cols-1;j>=1;j--) {
            if (matrix[i][0]==0 || matrix[0][j]==0) {
                matrix[i][j]=0;
            }
        }
        //借助flag，将matrix[i][0]=0结果传下来
        //方便j层循环中的遍历
        if (flag==0) matrix[i][0]=0;
    }

}
```