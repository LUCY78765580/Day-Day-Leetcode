## 062 Unique Paths
[题目地址](https://leetcode.com/problems/maximum-subarray/description/)
<br>
<br>
<br>

![](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/array001.jpg)
<br>

```c
//求唯一路径

//方法一：找规律，套公式
//高中数学的排列组合问题,找规律用公式就行了
// Combination(N, k) = n! / (k!(n - k)!)
// C = ( (n - k + 1) * (n - k + 2) * ... * n ) / k!
//注意这里的K-N+i,可以说i非常巧妙了
int uniquePaths(int m,int n) {
    int N=m-1;int K=m+n-1;
    int i;int res=1;

    for (i=1;i<=N;i++) {
        res=res*(K-N+i)/i;
    }

    return res;
}


//方法二：使用数组，动态规划
//动态规划就是一个有关状态的算法，某一步的结果与之前有关，但是又不需要记录之前所有每一步的过程，只需要记前一步的结果
//总公式：dp[i][j]=dp[i-1][j]+dp[i][j-1];

int uniquePaths(int m,int n) {
    //注意题干，m,n最大是100
    int dp[100][100];

    //首先要对数组进行初始化的（特别注意）
    for (int i=0;i<m;i++) dp[1][0]=1;
    for (int j=0;j<n;j++) dp[0][j]=1;

    for (int i=1;i<m;i++) {
        for (int j=1;j<n;j++) {
            dp[i][j]=dp[i-1][j]+dp[i][j-1];
        }
    }

    //这里dp[i]中i代表下标值
    //所以用m-1和n-1
    return dp[m-1][n-1];
}
```
