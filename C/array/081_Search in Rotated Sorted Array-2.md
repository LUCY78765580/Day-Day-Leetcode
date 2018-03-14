## 081 Search in Rotated Sorted Array II
[题目地址](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/description/)
<br>
<br>
<br>

```c
//搜索旋转排序数组

//这个题是前面033 Search in Rotated Sorted Array
//唯一不同的是本题中可能数组中有重复数值
//本题中只需要判断target是否存在，不需要返回下标值


//方法一：暴力解决（枚举法）
//时间复杂度O（n）
bool search(int* nums, int numsSize, int target) {
    int flag=0;
    //if (!nums || numsSize==0) flag=-1;

    if (target==nums[0]) flag=1;
    if (target==nums[numsSize-1]) flag=1;

    if (target>nums[0]) {
        for (int i=1;i<numsSize-1&&nums[i]>=nums[i-1];i++) {
            if (target==nums[i]) {
                flag=1;
                break;
            }
        }
    }
    else if (target<nums[0]) {
        for (int j=numsSize-2;j>0&&nums[j]<=nums[j+1];j--) {
            if (target==nums[j]) {
                flag=1;
                break;
            }
        }
    }
    return flag;
}



//方法二：二分法
//参考博客（讲的挺好）：http://blog.csdn.net/linhuanmars/article/details/20588511
/*
原来我们是依靠中间和边缘元素的大小关系，来判断哪一半是不受rotate影响，仍然有序的。
而现在因为重复的出现，如果我们遇到中间和边缘相等的情况，我们就丢失了哪边有序的信息，
因为哪边都有可能是有序的结果。假设原数组是{1,2,3,3,3,3,3}，那么旋转之后有可能是{3,3,3,3,3,1,2}，或者{3,1,2,3,3,3,3}，
这样的我们判断左边缘和中心的时候都是3，如果我们要寻找1或者2，我们并不知道应该跳向哪一半。

解决的办法  只能是对边缘移动一步，直到边缘和中间不在相等或者相遇，

这就导致了会有不能切去一半的可能。所以最坏情况（比如全部都是一个元素，或者只有一个元素不同于其他元素，而他就在最后一个）
就会出现每次移动一步start++，总共是n步，算法的时间复杂度变成O(n)，空间复杂度O(1)

*/
bool search(int* nums, int numsSize, int target) {
    int start=0;
    int end=numsSize-1;

    while (start<=end) {
        int mid=(start+end)/2;
        if (target==nums[mid]) return true;

        //1)左边边有序（升序）
        //判断target是否在start、mid之间
        //target>=nums[start]&&target<nums[mid]
        if (nums[start]<nums[mid]) {
            if (target>=nums[start]&&target<nums[mid]) {
                end=mid-1;
            }
            else {
                start=mid+1;
            }
        }

        //2)右边有序（升序）
        //判断target是否在mid、end之间
        //target>nums[mid]&&target<=nums[end]
        else if (nums[start]>nums[mid]) {
            if (target>nums[mid]&&target<=nums[end]) {
                start=mid+1;
            }
            else {
                end=mid-1;
            }
        }

        //3)遇到重复的
        //就是中间和边缘相等时，边缘移动一步
        else {
            start++;
        }
    }
    return false;
}
```
