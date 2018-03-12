## 053 Maximum Subarray
[题目地址](https://leetcode.com/problems/maximum-subarray/description/)
<br>
<br>
<br>

```c
//非常经典的"最大和子序列问题"

//方法一：二分法,复杂度O(nlog(n))
//计算3个数中的最大值
int Max3(int a,int b,int c) {
    int temp1,temp2;
    temp1=(a>b)?a:b;
    temp2=(temp1>c)?temp1:c;

    return temp2;

}

//二分法+递归,共有三种可能的情况
//1）左边加起来有最大值
//2）右边加起来有最大值
//3）最大值为左右两边相加（就是范围跨左右）

int maxSub(int* A,int left,int right) {
    int maxLeft,maxRight;
    int leftBorder,rightBorder;
    int maxLeftBorder,maxRightBorder;
    int mid,i;

    //仅有一个数的时候，left==right
    if (left==right) return A[left];

    //否则求mid,并递归求解左右边
    mid=(left+right)/2;
    maxLeft=maxSub(A,left,mid);
    maxRight=maxSub(A,mid+1,right);

    maxLeftBorder=INT_MIN;
    leftBorder=0;
    for (int i=mid;i>=left;i--) {
        leftBorder+=A[i];
        maxLeftBorder=maxLeftBorder>leftBorder?maxLeftBorder:leftBorder;
    }

    maxRightBorder=INT_MIN;
    rightBorder=0;
    for (int i=mid+1;i<=right;i++) {
        rightBorder+=A[i];
        maxRightBorder=maxRightBorder>rightBorder?maxRightBorder:rightBorder;
    }

    return Max3(maxLeft,maxRight,maxLeftBorder+maxRightBorder);
}

//总函数
int maxSubArray(int* nums,int numsSize) {
    return maxSub(nums,0,numsSize-1);
}





//方法二：联机算法,复杂度O(n)
int maxSubArray(int* nums,int numsSize) {
    int thisSum=0;
    int maxSum=INT_MIN;

    for (int i=0;i<numsSize;i++) {
        thisSum+=nums[i];
        maxSum=maxSum>thisSum?maxSum:thisSum;

        if (thisSum<0) thisSum=0;
    }

    return maxSum;
}



//方法三：动态规划
//由题目可以推出：maxSubArray(A, i) = maxSubArray(A, i - 1) > 0 ? maxSubArray(A, i - 1) : 0 + A[i];
//其实就是上面联机算法的思想
//参考discuss：https://leetcode.com/problems/maximum-subarray/discuss/20193/DP-solution-and-some-thoughts

int maxSubArray(int* nums,int numsSize) {
    int dp[numsSize];
    dp[0]=nums[0];
    int maxSum=dp[0];

    for (int i=1;i<numsSize;i++) {
        dp[i]=nums[i]+((dp[i-1]>0)?dp[i-1]:0);
        maxSum=maxSum>dp[i]?maxSum:dp[i];
    }

    return maxSum;
}
```