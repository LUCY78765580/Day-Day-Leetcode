## 088 Merge Sorted Array
[题目地址](https://leetcode.com/problems/merge-sorted-array/description/)
<br>
<br>
<br>

```c
//合并排序数组

//主要策略：这里从数组的尾部下手,将nums2并到nums1中
//注意很有可能nums1已经比较完了，但是nums2还没有完全放入，所以要加上后面的while(j>=0)

void merge(int* nums1, int m, int* nums2, int n) {
    //i,j,k分别代表三个数组的下标
    int i=m-1;
    int j=n-1;
    int k=m+n-1;

    while (i>=0&&j>=0) {
        nums1[k--]=(nums1[i]>nums2[j])?nums1[i--]:nums2[j--];
    }
    while (j>=0) {
        nums1[k--]=nums2[j--];
    }

}
```