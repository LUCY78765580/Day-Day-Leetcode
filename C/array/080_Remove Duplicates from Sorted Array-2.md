## 080 Remove Duplicates from Sorted Array II
[题目地址](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/description/)
<br>
<br>
<br>

```c
//这里是 026 Remove Duplicates from Sorted Array 的扩展
//每一个数字最多可以重复2次（不可以重复超过3次）

//方法一：枚举法（列举所有的可能性）
int removeDuplicates(int* nums,int numsSize) {
    int count=0;

    for (int i=0;i<numsSize;i++) {
        if (i<2 || nums[i]!=nums[i-1] || nums[i]!=nums[count-2]) {
            nums[count++]=nums[i];
        }
    }

    return count;
    //count代表的是i所在的下标位置
    //因为每一次最后都是count++，最后一个也加了count++,所以就不用再加1
}


//方法二：利用Sorted Array这个条件
//参考discuss:https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/discuss/27976/3-6-easy-lines-C++-Java-Python-Ruby
int removeDuplicates(int* nums,int numsSize) {
    int count=0;

    for (int i=0;i<numsSize;i++) {
        //或者这里nums[i]!=nums[count-1]
        if (count<2 || nums[i]>nums[count-2] ) {
            nums[count++]=nums[i];
        }
    }

    return count;
}
```
