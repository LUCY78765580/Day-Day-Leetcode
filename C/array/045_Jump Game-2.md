## 045  Jump Game II
[题目地址](https://leetcode.com/problems/jump-game-ii/description/)
<br>
<br>
<br>

```c
//跳跃游戏

//方法同055  Jump Game相似，使用贪心策略
//Your goal is to reach the last index in the minimum number of jumps.
//就是使得步数最小，求出最小步数
//特别注意：题意要求不一定非得跳到last index，越过去也算

int jump(int* nums, int numsSize) {
    int count=0; //跳过次数
    int last=0;  //上一跳可达最远距离
    int reach;   //当前可到达最远距离

    for (int i=0;i<numsSize-1;i++) {
        reach=(reach>(i+nums[i]))?reach:(i+nums[i]);
        if (i==last) {
            count++;
            last=reach;
        }
    }

    return count;
}
```