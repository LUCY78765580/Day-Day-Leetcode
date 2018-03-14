## 033 Search in Rotated Sorted Array
[题目地址](https://leetcode.com/problems/search-in-rotated-sorted-array/description/)
<br>
<br>
<br>

```c
//在旋转有序数组中搜索
//这个题目是 035 Search Insert Position 的变式

/*
审题：
1) 数组升序排列，中间可能有翻转
2) 没有重复元素
3) 给定一个target查找，找到返回index,否则返回-1
*/


//方法一：暴力破解（枚举法）
//哈哈自己想出来的，分两头遍历，时间复杂度 O（n）

int search(int* nums,int numsSize,int target) {
    //设置一个flag标志
    int flag=-1;

    if (target==nums[0]) flag=0;
    if (target==nums[numsSize-1]) flag=numsSize-1;

    //如果target>nums[0]，从左到右遍历,nums[i]>nums[i-1]，一直遇到分割点
    if (target>nums[0]) {
        for (int i=1;i<numsSize-1&&nums[i]>nums[i-1];i++) {
            if (target==nums[i]) {
                flag=i;
                break;
            }
        }
    }
    //如果target<nums[0],从右边到左边遍历，nums[j]<nums[j+1]，一直遇到分割点
    else {
        for (int j=numsSize-2;j>=1&&nums[j]<nums[j+1];j--) {
            if (target==nums[j]) {
                flag=j;
                break;
            }
        }
    }

    return flag;
}


//方法二：二分法
//每次都可以切掉一半数据，算法的时间复杂度是O(logn)，空间复杂度是O(1)
/*
分搜索法的关键在于获得了中间数后，判断下面要搜索左半段还是右半段
如果中间的数小于最右边的数，则右半段是有序的，若中间数大于最右边数，则左半段是有序的

（1）如果target==A[m]，那么m就是我们要的结果，直接返回；
（2）如果A[m]<A[r]，那么说明从m到r(右边)一定是有序的（没有受到rotate的影响），那么我们只需要判断target是不是在m到r之间，如果是则把左边缘移到m+1，否则就target在另一半，即把右边缘移到m-1。
（3）如果A[m]>=A[r]，那么说明从l到m(左边)一定是有序的，同样只需要判断target是否在这个范围内，相应的移动边缘即可。
*/

int search(int* nums, int numsSize, int target) {
    int start=0;
    int end=numsSize-1;
    int flag=-1;

    while (start<=end) {
        int mid=(start+end)/2;
        if (target==nums[mid]) flag=mid;

        //左边有序（升序）
        //判断target是否在start、mid之间
        if (nums[start]<=nums[mid]) {
            if (target>=nums[start]&&target<=nums[mid]) {
                end=mid-1;
            }
            else {
                start=mid+1;
            }
        }
        //右边有序（升序）
        //判断target是否在mid、end之间
        else {
            if (target>=nums[mid]&&target<=nums[end]) {
                start=mid+1;
            }
            else {
                end=mid-1;
            }
        }
    }
    return flag;
}
```