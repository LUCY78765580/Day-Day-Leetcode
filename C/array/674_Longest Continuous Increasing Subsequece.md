## 674  Longest Continuous Increasing Subsequence
[题目地址](https://leetcode.com/problems/longest-continuous-increasing-subsequence/description/)
<br>
<br>
<br>

```c
//求最长连续增长子序列长度，典型问题，又被叫做LCIS

//这个题与前面的 053 Maximum Subarray (最大和子序列问题)，非常相似
//单调递增，就是nums[i]>nums[i-1]


//方法一：一般方法
int findLengthOfLCIS(int* nums,int numsSize) {
    if (nums==NULL||numsSize==0) return 0;
    int thisLen=1;int maxLen=1;

    //开始的时候，处在第一个位置，thisLen为1
    //从第二个位置开始循环

    for (int i=1;i<numsSize;i++) {
        if (nums[i]>nums[i-1]) thisLen++;
        else                   thisLen=1;

        maxLen=(maxLen>thisLen)? maxLen:thisLen;
    }

    return maxLen;
}


//方法二：动态规划
//用dp[i]记录当前序列长度
//本质上和上面是一样的Orz

//得不到正确答案？？不是很理解了
int findLengthOfLCIS(int* nums,int numsSize) {
    if (nums==NULL || numsSize==0) return 0;

    int dp[numsSize];
    dp[0]=1;
    int max=1;

    for (int i=1;i<numsSize;i++) {
        if (nums[i]>nums[i-1]) {
            dp[i]=dp[i-1]+1;
        } else {
            dp[i]=0;
        }

        max=(max>dp[i])? max:dp[i];
    }

    return max;
}
```