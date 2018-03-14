## 122  Best Time to Buy and Sell Stock II
[题目地址](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/description/)
<br>
<br>
<br>

```c
//买卖股票的最佳时间-2

//这一题是前面 121  Best Time to Buy and Sell Stock 的变式
//给一个数组prices[]，prices[i]代表股票在第i天的售价，求出只做一次交易(一次买入和卖出)能得到的最大收益。

/*
这道题由于可以无限次买入和卖出(当然买和卖不能在同一天的)。
我们都知道炒股想挣钱当然是低价买入高价抛出，那么这里我们只需要从第二天开始，如果当前价格比之前价格高，则把差值加入利润中，
因为我们可以昨天买入，今日卖出，若明日价更高的话，还可以今日买入，明日再抛出。

以此类推，遍历完整个数组后即可求得最大利润

//理解这个题目的意思就好说了
//比如[0,3,5,7] （0，7）和(0,3)+(3,5)+(5,7)效果其实是一样的，所以综上总的结果可以是所有a[i+1]-a[i]的和
*/


int maxProfit(int* prices, int pricesSize) {
    int maxPro=0;

    for (int i=1;i<pricesSize;i++) {
        int temp=prices[i]-prices[i-1];
        if (temp>0) {
            maxPro+=temp;
        }
    }

    return maxPro;
}
```