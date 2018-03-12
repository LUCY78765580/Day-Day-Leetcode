## 074  Search a 2D Matrix
[题目地址](https://leetcode.com/problems/search-a-2d-matrix/description/)
<br>
<br>
<br>

```c

//在2D矩阵中查找数
//因为数据有规则的由小到大排列，所以比较简单了


//方法一：暴力破解法
bool searchMatrix(int** matrix, int matrixRowSize, int matrixColSize, int target) {
    int m=matrixRowSize;
    int n=matrixColSize;
    int row,col;
    int flag=0;

    if (m!=0&&n!=0&&target>=matrix[0][0]&&target<=matrix[m-1][n-1]) {
        //从最后一行开始，第0列开始排除，找到某一行row
        for (row=m-1;row>=0&&target<matrix[row][0];row--) ;

        //在第row行查找
        for (col=0;col<=n-1;col++) {
            if (target==matrix[row][col]) flag=1;
        }
    }
    return flag;
}



//方法二：分治法
//将矩阵看作是一个升序排列的数组即可
//关键点：mid所在位置的值 mid_value=matrix[mid/n][mid%n]

bool searchMatrix(int** matrix,int matrixRowSize,int matrixColSize,int target) {
    int m=matrixRowSize;
    int n=matrixColSize;
    int start=0;
    int end=m*n-1;

    while (start<=end) {
        int mid=(start+end)/2;
        int mid_value=matrix[mid/n][mid%n];

        if (mid_value==target) return true;
        else if (mid_value>target) {
            end=mid-1;
        }
        else {
            start=mid+1;
        }
    }

    return false;
}
```