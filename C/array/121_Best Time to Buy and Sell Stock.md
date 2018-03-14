## 121  Best Time to Buy and Sell Stock
[题目地址](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/)
<br>
<br>
<br>

```c
//买卖股票的最佳时间

//给一个数组prices[]，prices[i]代表股票在第i天的售价，求出只做一次交易(一次买入和卖出)能得到的最大收益。
//本质上此题思路是two pointers的思路，在固定当前pointer的情况下，考虑另外一个pointer能够取得的最优值

//方法一：暴力破解（这里是双重循环）
//时间复杂度 O（n*n),空间复杂度：O（1）
int maxProfit(int* prices, int pricesSize) {
    int i,j;
    int maxPro=0;
    int temp;

    for (i=0;i<pricesSize-1;i++) {
        for (j=i+1;j<pricesSize;j++) {
            temp=prices[j]-prices[i];
            if (temp>0) {
                maxPro=(maxPro>temp)?maxPro:temp;
            }
        }
    }
    return maxPro;
}



//方法二:添加参数min，代表买入最低价
//方法类似于 “接雨水”那一题，042  Trapping Rain Water
//参考博客：http://blog.csdn.net/feliciafay/article/details/18578441
//时间复杂度O(n),空间复杂度O(1)
int maxProfit(int* prices, int pricesSize) {
    int maxPro=0;
    int min=prices[0];
    int temp;

    if (pricesSize==0) return NULL;
    for (int i=0;i<pricesSize;i++) {
        min=(min<prices[i])?min:prices[i];
        temp=prices[i]-min;
        if (temp>0) {
            maxPro=(maxPro>temp)?maxPro:temp;
        }
    }
    return maxPro;
}


//方法三：动态规划
//参考博客：http://bookshadow.com/weblog/2015/11/24/leetcode-best-time-to-buy-and-sell-stock-with-cooldown/

```