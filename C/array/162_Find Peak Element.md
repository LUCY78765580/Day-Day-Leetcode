## 162 Find Peak Element
[题目地址](https://leetcode.com/problems/find-peak-element/description/)
<br>
<br>
<br>

```c
//查找峰元素

//注意这个题目：peak值是峰值（并不是最大值）peak元素可以有很多个
//峰值特点：相邻位置不同；A[p]>A[p-1] A[p]>A[p+1]


//方法一：暴力解决（枚举法）
int findPeakElement(int* nums,int numsSize) {
    if (!nums || numsSize==0) return NULL;
    if (numsSize==1) return 0;
    if (nums[0]>nums[1]) return 0;
    if (nums[numsSize-1] > nums[numsSize-2]) return numsSize-1;

    int i;
    for (i=1;i<numsSize-1;i++) {
        if (nums[i]>nums[i-1]&&nums[i]>nums[i+1]) {
            break;
        }
    }

    return i;
}



//方法二：二分查找
//这里有点难理解了

int findPeakElement(int* nums, int numsSize) {
    int start=0;
    int end=numsSize-1;

    while (start<end) {
        int mid=(start+end)/2;

        if (nums[mid]<nums[mid+1]) {
            start=mid+1;
        }
        else {
            end=mid;
        }
    }

    //最后返回return start或者return end都是一样的效果
    return end;
}
```
