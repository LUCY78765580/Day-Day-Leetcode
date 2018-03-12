## 167 Two Sum II - Input array is sorted
[题目地址](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/description/)
<br>
<br>
<br>

```c
/**
 * Return an array of size *returnSize.
 * Note: The returned array must be malloced, assume caller calls free().
 */

//两数之和
//这一题是 001 Two Sum的变形
//注意这里数组已经是排好序的了


//方法一：一般的双重循环法
int* twoSum(int* numbers, int numbersSize, int target, int* returnSize) {
    //数组的长度是 *returnSize,需要给它malloc分配内存
    //不要误解为*returnSize就是返回的数组了

    int* res=(int*)malloc(sizeof(int)*2);
    int i,j;

    for (i=0;i<numbersSize;i++) {
        for (j=i+1;j<numbersSize;j++) {
            if (numbers[i]+numbers[j]==target) {
                //注意这里的(both index1 and index2) are not zero-based.
                //最后的i,j都要加1
                res[0]=i+1;
                res[1]=j+1;

                //最后的*returnSize,在返回res之前，需要重新设置值
                //这一步是非常重要的,不设置，会返回一个空数组
                *returnSize=2;
                goto FOUND;
            }
        }
    }
    FOUND:
    return res;
}


//方法二：使用双指针
int twoSum(int* numbers,int numbersSize,int target,int* returnSize) {
    int i=0;int j=numbersSize-1;
    int* res=(int*)malloc(2*sizeof(int));

    while (i<j) {
        int temp=numbers[i]+numbers[j];
        if (temp==target) {
            res[0]=i+1;
            res[1]=j+1;
            *returnSize=2;
            return res;
        }
        else if (temp<target) i++;
        else                  j--;
    }

    return res;
}



//方法三：二分查找(与方法二类似)
//有一点烦的，复杂度O(nlog(n))
//参考discuss：https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/discuss/51268/A-less-efficient-way-(binary-search)

int twoSum(int* numbers,int numbersSize,int target,int* returnSize) {
    int* res=(int*)malloc(2*sizeof(int));

    //注意这里的left=i+1
    //这里不是很理解了
    //同时注意 left<=right
    for (int i=0;i<numbersSize;i++) {
        int left=i+1;int right=numbersSize-1;
        int temp=target-numbers[i];

        while (left<=right) {
            int mid=(left+right)/2;
            if (temp==numbers[mid]) {
                res[0]=i+1;
                res[1]=mid+1;
                *returnSize=2;
                return res;
            }
            else if (temp < numbers[mid]) right=mid-1;
            else                           left=mid+1;
        }
    }

    return res;
}



//方法四：使用hashMap并查集

```