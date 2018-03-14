## 041 First Missing Positive
[题目地址](https://leetcode.com/problems/first-missing-positive/description/)
<br>
<br>
<br>

```c
//首个缺失的正数

//our algorithm should run in O(n) time and uses constant space.
//题目规定 时间复杂度O(n)，空间复杂度O(1)
//这道题要求用线性时间和常量空间，思想借鉴到了Counting sort中的方法

/*
Counting Sort:
  利用数组的index来作为数字本身的索引，把正数按照递增顺序依次放到数组中。
即让A[0]=1, A[1]=2, A[2]=3, ... , 这样一来，最后如果哪个数组元素违反了A[i]=i+1即说明i+1就是我们要求的第一个缺失的正数。
对于那些不在范围内的数字，我们可以直接跳过，比如说负数，0，或者超过数组长度的正数，这些都不会是我们的答案

  实现中还需要注意一个细节，就是如果当前的数字所对应的下标已经是对应数字了(nums[i]==i+1)，那么我们也需要跳过，
因为那个位置的数字已经满足要求了，否则会出现一直来回交换的死循环

参考博客1：http://blog.csdn.net/linhuanmars/article/details/20884585
参考博客2：http://www.cnblogs.com/AnnieKim/archive/2013/04/21/3034631.html
*/

//还是比较难理解
void swap(int* nums,int i,int j) {
    int temp=nums[i];
    nums[i]=nums[j];
    nums[j]=temp;
}

int firstMissingPositive(int* nums, int numsSize) {
    int i=0;
    int j=0;

    while (i<numsSize) {
        //注意范围是：正整数（不包括负数、0）
        if (nums[i]==i+1 || nums[i]<=0 || nums[i]>numsSize) i++;

        //绕过来其实是 nums[nums[i]-1] == (nums[i]-1) + 1
        //就是如果不满足模型nums[i]=i+1，则进行交换，知道数据满足排序
        else if (nums[i]!=nums[nums[i]-1]) {
            swap(nums,i,nums[i]-1);
        }
        else {
            i++;
        }
    }

    while (j<numsSize&&nums[j]==j+1) j++;
    return j+1;
}


//简写也可以写成下面这样
int firstMissingPositive(int* nums, int numsSize){
    int i=0;
    int j=0;

    while (i<numsSize) {
        if (nums[i]!=nums[nums[i]-1]&&nums[i]!=i+1&&nums[i]>0&&nums[i]<=numsSize) {
            swap(nums,i,nums[i]-1);
        }
        else i++;
    }

    while (j<numsSize&&nums[j]==j+1) j++;
    return j+1;
}
```