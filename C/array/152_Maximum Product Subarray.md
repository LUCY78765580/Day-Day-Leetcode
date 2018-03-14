## 152 Maximum Product Subarray
[题目地址](https://leetcode.com/problems/maximum-product-subarray/description/)
<br>
<br>
<br>

```c
//最大乘积连续子数组
//这一题相当于前面 053 Maximum Subarray 的变式

//首先要保证数组是连续的
//其次保证乘积最大
//参考博客：https://www.cnblogs.com/bakari/p/4007368.html


//方法一：暴力破解（两个指针）
//得不到正确答案
int maxProduct(int* nums, int numsSize) {
    int maxPro=INT_MIN;

    for (int i=0;i<numsSize;i++) {
        for (int j=i+1;j<numsSize;j++) {
            maxPro=maxPro>(nums[i]*nums[j])?maxPro:(nums[i]*nums[j]);
        }
    }

    return maxPro;
}




//方法二:维护一个最大值、一个最小值
//时间复杂度O(n),空间复杂度O(1)
//实际上也是动态规划
//参考discuss:https://leetcode.com/problems/maximum-product-subarray/discuss/48261/Share-my-DP-code-that-got-AC
/*
应该注意到，数组中数据可正可负，所以处理起来比较难办。
  正数乘以正数为正数，负数乘以负数为正数。
  比如当前元素值为正数，则正数才会更大。即出现更大的值，如果出现负数，则乘以负数才会得到更大的值。
所以我们需要记录两个值，一个是最大值，一个是最小值。
  初始情况，最大值，最小值均为第一个元素。然后向后遍历数组，分别和最大值和最小值相乘得到结果，
然后最大值和两者乘积结果中的最大值对比更新，然后最小值和乘积结果中的最小值对比更新。

参考博客：http://blog.csdn.net/sjwl2012/article/details/54576777
*/
//还是有点绕。。。

int maxProduct(int* nums, int numsSize) {
    int minPro=nums[0];
    int maxPro=nums[0];
    int res=nums[0];
    int temp1,temp2;

    //temp1、temp2是新的maxPro、minPro；
    //同时maxPro、minPro还要同当前nums[i]比较
    //最后 更新maxPro
    for (int i=1;i<numsSize;i++) {
        int t1=minPro*nums[i];
        int t2=maxPro*nums[i];
        temp1=t1>t2?t1:t2;
        temp2=t1<t2?t1:t2;

        maxPro=temp1>nums[i]?temp1:nums[i];
        minPro=temp2<nums[i]?temp2:nums[i];
        res=maxPro>res?maxPro:res;
    }

    return res;
}
```