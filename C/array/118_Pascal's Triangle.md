## 118  Pascal's Triangle
[题目地址](https://leetcode.com/problems/pascals-triangle/description/)
<br>
<br>
<br>

```c
/**
 * Return an array of arrays.
 * The sizes of the arrays are returned as *columnSizes array.（注意理解这句话）
 * Note: Both returned array and *columnSizes array must be malloced, assume caller calls free().
 */


//典型的杨辉三角（又叫帕斯卡三角）

//博客参考：http://www.cnblogs.com/clover-toeic/p/3766001.html
//行数和列数都相等,一个重要性质：a[i][j]=a[i-1][j-1]+a[i-1][j] (从第３行第２列开始)
//审题:要重新malloc的

int** generate(int numRows, int** columnSizes) {
    int** res=(int**)malloc(sizeof(int*)*numRows);
    *columnSizes=(int*)malloc(sizeof(int)*numRows);

    int i,j;
    for (i=0;i<numRows;i++) {
        res[i]=(int*)malloc(sizeof(int)*(i+1));
        (*columnSizes)[i]=i+1;

        //每一行长度为i+1
        //特别要注意！！！这里的*columnSizes，一定要加上括号！！

        for (j=0;j<i+1;j++) {
            if (j==0||j==i) {
                res[i][j]=1;
            }
            else {
                res[i][j]=res[i-1][j-1]+res[i-1][j];
            }
        }
    }

    //在这之前，不要忘记处理　**columnSizes;
    //**columnSizes是多维数组，*columnSizes是一个数组，存放着每一列的长度
    return res;
}




//更简洁的做法
int** generate(int numRows, int** columnSizes) {
    int** res=(int**)malloc(sizeof(int*)*numRows);
    *columnSizes=(int*)malloc(sizeof(int)*numRows);
    int i,j;

    for (i=0;i<numRows;i++) {
        res[i]=(int*)malloc(sizeof(int)*(i+1));
        (*columnSizes)[i]=i+1;
        res[i][0]=1;
        res[i][i]=1;

        //j=1，表示从第2列开始 (第3行第2列　　　)
        //j<i,而非j<i+1 =>　0和i都是边界，j只是中间位置不可能为边界

        for (j=1;j<i;j++) {
            res[i][j]=res[i-1][j-1]+res[i-1][j];
        }
    }

    return res;
}
```