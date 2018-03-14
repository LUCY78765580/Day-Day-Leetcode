## 128 Longest Consecutive Sequence
[题目地址](https://leetcode.com/problems/longest-consecutive-sequence/description/)
<br>
<br>
<br>


```c
//乱序数组之中的连续子数组长度

//方法一：暴力解决（快排+遍历）
//时间复杂度O(nlogn)，空间复杂度O(1)，不满足题目中的O(n)的复杂度



//方法二：HashMap或者set
/*
用hash表来解决这个问题，先初始化一个hash表， 存储所有数组元素，
然后遍历这个数组， 对找到的数组元素， 去搜索其相连的上下两个元素是否在hash表中，
如果在， 删除相应元素并增加此次查找的数据长度， 如果不在， 从下一个元素出发查找。

参考博客1：http://blog.csdn.net/qq508618087/article/details/50734884
参考博客2：https://segmentfault.com/a/1190000003812046
*/

//C语言建Hash_Map实在是。。。。


```
