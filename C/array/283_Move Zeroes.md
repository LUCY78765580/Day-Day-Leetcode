## 283  Move Zeroes
[题目地址](https://leetcode.com/problems/move-zeroes/description/)
<br>
<br>
<br>

```c
//移动数组中的零

//这个题目比较简单
//与前面的026 Remove Duplicates from Sorted Array、027 Remove Element类似
void moveZeroes(int* nums,int numsSize) {
    int count=0;

    for (int i=0;i<numsSize;i++) {
        if (nums[i]!=0) {
            nums[count++]=nums[i];
        }
    }

    while (count<numsSize) {
        nums[count++]=0;
    }
}
```