## 153 Find Minimum in Rotated Sorted Array
[题目地址](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/description/)
<br>
<br>
<br>

```c
//在旋转有序数组中找最小值

//这个题目和之前 033 Search in Rotated Sorted Array 类似,解法也类似
//最要注意的：数组已经是排序好了,没有重复数字
//注意如果是在i=0位置rotate,数组不发生变化


//方法一：暴力解决（枚举法）
//时间复杂度O(n)，空间复杂度O(1)
int findMin(int* nums, int numsSize) {
    int min,i;

    if (!nums || numsSize==0) return NULL;
    if (numsSize==1) min=nums[0];

    //因为没有重复数字，nums[0]不可能等于nums[numsSize-1]
    if (nums[0]<nums[numsSize-1]) {
        min=nums[0];
    }
    else if (nums[0]>nums[numsSize-1]) {
        for (i=0;i<numsSize-1&&nums[i]>nums[i-1];i++) {
            ;
        }
        min=nums[i];
    }

    return min;
}



//方法二：二分法+递归
//这里和前面“最大和子序列” 053 Maximum Subarray很像
//时间复杂度O(nlgn)，空间复杂度O(1)
int Min3(int a,int b,int c) {
    int temp1,temp2;

    temp1=(a<b)?a:b;
    temp2=(temp1<c)?temp1:c;
    return temp2;
}

int findSubMin(int* nums,int start,int end) {
    int startMin,endMin;

    if (start==end) return nums[start];
    int mid=(start+end)/2;

    startMin=findSubMin(nums,start,mid);
    endMin=findSubMin(nums,mid+1,end);
    return Min3(startMin,endMin,nums[mid]);
}

int findMin(int* nums,int numsSize) {
    return findSubMin(nums,0,numsSize-1);
}


//方法三：二分法+简化版
//处理边界还是很麻烦的
//时间复杂度O(lgn),空间复杂度O(1)
int findMin(int* nums, int numsSize) {
    int start=0;
    int end=numsSize-1;

    while (start<end) {
        int mid=(start+end)/2;

        //注意这里找的是最小值
        //nums[mid]大于nums[end]，说明最小值在最右边
        //反之，最小值在最左边
        if (nums[mid]>nums[end]) {
            start=mid+1;
        }
        else {
            end=mid;
        }
    }

    return nums[start];
    //这里返回nums[start]或者nums[end]都是可以的
}
```