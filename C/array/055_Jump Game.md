## 055  Jump Game
[题目地址](https://leetcode.com/problems/jump-game/description/)
<br>
<br>
<br>


```c
//跳跃游戏

//注意看好题目：每一个数字代表所能跳的最大的数量（maximun）
//注意这里的maximum,比如[2,5,0,0]可以先跳一格到5，然后2格到最后
/*
这道题的思路就是使用一个贪心算法，使用一个步进指针，用一个上界指针。
每次遍历的时候，不停的更新上界指针的位置（也就是当前位置+当前可以跳到的位置），
如果最后能遇到结尾，就范围true，没有就返回false
*/

//方法一：贪心
//关键点：求最大的reach（i+nums[i]），同时严格i<=reach,i++
bool canJump(int* nums, int numsSize) {
    int i=0;

    for (int reach=0;i<numsSize&&i<=reach;i++) {
        reach=(reach>(i+nums[i]))?reach:(i+nums[i]);
    }

    return (i==numsSize);
}


//方法二：回溯
//仍是贪心，本质同方法一是一样的
bool canJump(int* nums,int numsSize) {
    int last=numsSize-1;
    int i,j;

    for (i=numsSize-2;i>=0;i--) {
        if (i+nums[i]>=last) last=i;
    }
    return last<=0;
}
```