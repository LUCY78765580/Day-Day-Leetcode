## 643 Maximum Average Subarray I
[题目地址](https://leetcode.com/problems/maximum-average-subarray-i/description/)
<br>
<br>
<br>

```c
//最大子阵列平均数

//给定一个由n个整数组成的数组，找到具有最大平均值的给定长度为k的连续子数组。 你需要输出最大的平均值。
//采用 Sliding window模型(实际上是动态规划)
//参考1：http://blog.csdn.net/Ellie_/article/details/76781877
//参考2：https://stackoverflow.com/questions/8269916/what-is-sliding-window-algorithm-examples


double findMaxAverage(int* nums, int numsSize, int k) {
    int sum=0;
    int maxSum=0;

    for (int i=0;i<k;i++) {
        maxSum+=nums[i];
    }

    //前面已经将k个数像加
    //关键：sum=sum+nums[i]-nums[i-k]
    //比如[1,12,-5,-6]和[12,-5,-6,50]
    //第一次nums[i]=nums[4]=50; nums[i-k]=nums[0]=1,sum=maxSum-1+50
    //第二次nums[i]=nums[5]=3,nums[i-k]=nums[1]=12,sum=maxSum-12+3
    sum=maxSum;
    for (int i=k;i<numsSize;i++) {
        sum=sum-nums[i-k]+nums[i];
        maxSum=maxSum>sum?maxSum:sum;
    }

    return (double)maxSum/k;
}

```