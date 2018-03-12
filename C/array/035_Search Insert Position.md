## 035 Search Insert Position
[题目地址](https://leetcode.com/problems/search-insert-position/description/)
<br>
<br>
<br>

```c
//查找插入位置

//一般的方法
int searchInsert(int* nums,int numsSize,int target) {
    if (nums==NULL) return 0;
    int i;

    for (i=0;i<numsSize;i++) {
        if (nums[i]>=target)
            break;
    }
    return i;
}

//简化版本如下
//nums[i]<=target的时候循环结束
int searchInsert(int* nums,int numsSize,int target) {
    int i=0;

    for ( ;i<numsSize&&nums[i]<target;i++);
    return i;
}

```