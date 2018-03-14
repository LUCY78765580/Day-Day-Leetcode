## 075  Sort Colors
[题目地址](https://leetcode.com/problems/sort-colors/description/)
<br>
<br>
<br>

```c
//颜色排序

//用冒泡排序超级简单
//题目不允许使用library's sort (参考博客：http://www.cnblogs.com/richselian/archive/2011/09/16/2179152.html)

//方法一：排序，simple and lazy
//冒泡，时间复杂度 O（n^2）
//当然也可以用快速排序，时间复杂度O（nlog(n)）
void sortColors(int* nums, int numsSize) {
    int p,i;
    int temp;
    int flag;

    for (p=numsSize-1;p>=0;p--) {
        flag=0;
        for (i=0;i<p;i++) {
            if (nums[i]>nums[i+1]) {
                temp=nums[i+1];
                nums[i+1]=nums[i];
                nums[i]=temp;
                flag=1;
            }
        }
        if (flag==0) break;
    }
}


//方法二：使用两个指针，并且交换数据
//哈哈其实和快速排序分割数组也是有些类似
void sortColors(int* nums, int numsSize) {
    int p1=0;
    int p2=numsSize-1;
    int index;

    while (index<=p2) {
        //如果nums[index]为0，将nums[index]和nums[p1]位置交换
        //就是将0放到前面
        if (nums[index]==0) {
            nums[index]=nums[p1];
            nums[p1]=0;
            p1++;
        }

        //如果nums[index]为2，将nums[index]和nums[p2]位置交换
        //就是将2放到后面
        if (nums[index]==2) {
            nums[index]=nums[p2];
            nums[p2]=2;
            p2--;
            //注意这个地方
            //因为换过来的是“新的”值，之前没有遍历过，所以要index--然后index++保持不变
            index--;
        }

        index++;
    }
}
```