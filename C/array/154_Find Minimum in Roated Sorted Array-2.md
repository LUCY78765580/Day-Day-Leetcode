## 154 Find Minimum in Rotated Sorted Array II
[题目地址](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array-ii/description/)
<br>
<br>
<br>

```c
//在旋转有序数组中找最小值
//这一题是前面 153 Find Minimum in Rotated Sorted Array 的变式
//大体方法一样，只是这里允许数据重复，需要做少量改动


//方法一：暴力解决（枚举法）
//将nums[i]>nums[i-1]改为nums[i]>=nums[i-1]
//将nums[0]>nums[numsSize-1]改为nums[0]>=nums[numsSize-1]

int findMin(int* nums, int numsSize) {
    int min,i;

    if (!nums || numsSize==0) return NULL;
    if (numsSize==1) min=nums[0];

    if (nums[0]<nums[numsSize-1]) {
        min=nums[0];
    }
    //这里只能从后面遍历
    //从i=numsSize-1开始
    else if (nums[0]>=nums[numsSize-1]) {
        for (i=numsSize-1;i>=1&&nums[i]>=nums[i-1];i--) {
            ;
        }
        min=nums[i];
    }
    return min;
}



//方法二：二分＋递归（不用改动）
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

    return Min3(nums[mid],startMin,endMin);
}

int findMin(int* nums, int numsSize) {
   return findSubMin(nums,0,numsSize-1);
}




//方法三：二分法＋简化版
//当nums[mid]==nums[end]时，移动边缘end--（如果是左边则start++），使得end和mid不相遇
//参考博客：http://blog.csdn.net/linhuanmars/article/details/40449299

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
        else if (nums[mid]<nums[end]) {
            end=mid;
        }

        //当mid==end时候,end--
        else {
            end--;
        }
    }

    return nums[start];
    //这里返回nums[start]或者nums[end]都是可以的
}
```
