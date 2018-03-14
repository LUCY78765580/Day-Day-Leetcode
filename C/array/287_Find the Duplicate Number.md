## 287 Find the Duplicate Number
[题目地址](https://leetcode.com/problems/find-the-duplicate-number/description/)
<br>
<br>
<br>

```c
//查找重复数字

/*
审题：
只有一个数是重复的，但是可以重复多次
不能对数组进行修改，不能进行排序啊之类
时间复杂度小于O(n*2),空间复杂度O(1)
共有n+1个数，每一个数范围为1~n
*/


//方法一：快慢指针（借鉴了带环链表 142 Linked List Cycle II那一题）
//时间复杂度O(n),空间复杂度O(1)
//相遇后，头指针(head)到连接点(node)的距离 = 碰撞点(slow==fast时)到连接点(node)的距离

/*
基本思想是将数组抽象为一条线和一个圆环，因为1～nn 之间有n＋1个数，所以一定有重复数字出现，所以重复的数字即是圆环与线的交汇点。
然后设置两个指针，一个快指针一次走两步，一个慢指针一次走一步。
当两个指针第一次相遇时，令快指针回到原点（0）且也变成一次走一步，慢指针则继续前进，再次回合时即是线与圆环的交汇点。

参考discuss:https://leetcode.com/problems/find-the-duplicate-number/discuss/72846/My-easy-understood-solution-with-O(n)-time-and-O(1)-space-without-modifying-the-array.-With-clear-explanation.
参考博客1：http://blog.csdn.net/monkeyduck/article/details/50439840
参考博客2：http://bookshadow.com/weblog/2015/09/28/leetcode-find-duplicate-number/
*/

int findDuplicate(int* nums,int numsSize) {
    if (numsSize>1) {
        int slow=nums[0];
        int fast=nums[nums[0]];

        while (slow!=fast) {
            slow=nums[slow];
            fast=nums[nums[fast]];
        }

        fast=0;
        while (fast!=slow) {
            slow=nums[slow];
            fast=nums[fast];
        }

        return slow;
    }

    return -1;
}




//方法二：二分法(使用到Pigeonhole Principle 鸽巢原理/抽屉原理/重叠原理)
//时间复杂度O(nlogn),空间复杂度O(1)
//参考discuss:https://leetcode.com/problems/find-the-duplicate-number/discuss/72844/Two-Solutions-(with-explanation):-O(nlog(n))-and-O(n)-time-O(1)-space-without-changing-the-input-array

/*
鸽巢原理：不是很理解
参考博客：http://blog.csdn.net/thearcticocean/article/details/47009671
*/

int findDuplicate(int* nums,int numsSize) {
    int start=0;
    int end=numsSize-1;

    while (start<end) {
        int mid=(start+end)/2;
        int count=0;

        for (int i=0;i<numsSize;i++) {
            if (nums[i]<=mid) count++;
        }
        if (count<=mid) {
            start=mid+1;
        }
        else {
            end=mid;
        }
    }

    return start;
}
```