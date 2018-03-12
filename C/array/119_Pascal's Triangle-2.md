## 119  Pascal's Triangle II
[题目地址](https://leetcode.com/problems/pascals-triangle-ii/description/)
<br>
<br>
<br>

```c
/**
 * Return an array of size *returnSize.
 * Note: The returned array must be malloced, assume caller calls free().
 */

//返回帕斯卡三角形的某一行
//本题是 118  Pascal's Triangle 的变形


/*
这里我们只需要一行数据，就得考虑一下是不是能只用一行的空间来存储结果而不需要额外的来存储上一行呢？

这里确实是可以实现的。对于每一行我们知道如果从前往后扫，第i个元素的值等于上一行的res[i]+res[i+1]，
可以看到数据是往前看的，如果我们只用一行空间，那么需要的数据就会被覆盖掉。

所以这里采取的方法是从后往前扫，这样每次需要的数据就是res[i]+res[i-1]，
我们需要的数据不会被覆盖，因为需要的res[i]只在当前步用，下一步就不需要了。
这个技巧在动态规划省空间时也经常使用，主要就是看我们需要的数据是原来的数据还是新的数据来决定我们遍历的方向。

时间复杂度还是O(n^2)，而空间这里是O(k)来存储结果，仍然不需要额外空间
*/

int* getRow(int rowIndex, int* returnSize) {
    int* res=(int*)malloc(sizeof(int)*(rowIndex+1));
    int i,j;

    //对所有空格先设置为1
    for (i=0;i<rowIndex+1;i++) {
        res[i]=1;
    }

    //其它位置求值
    //j的值范围是1~(rowIndex-1)
    for (i=1;i<rowIndex;i++) {
        for (j=i;j>=1;j--) {
            res[j]+=res[j-1];
        }
    }

    //记得重新设置 *returnSize的值
    *returnSize=rowIndex+1;
    return res;
}

```