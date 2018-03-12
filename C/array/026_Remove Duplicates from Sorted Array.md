## 026 Remove Duplicates from Sorted Array
[题目地址](https://leetcode.com/problems/remove-duplicates-from-sorted-array/description/)
<br>
<br>
<br>


```c
//删掉排序数组中重复元素

//方法一：巧妙使用计数器count
int removeDuplicates(int* nums,int numsSize) {
    int i;
    int count=0;

    //如果nums为空
    if (numsSize<2) return numsSize;

    //如果nums不为空
    //不管怎样总是从i=2开始比较
    //计数器count总是要落后于i
    for (i=0;i<numsSize;i++) {
        if (nums[i]!=nums[count]) {
            nums[++count]=nums[i];
        }
    }

    //返回的是新数组的长度
    //而不是位置
    return ++count;
}
```