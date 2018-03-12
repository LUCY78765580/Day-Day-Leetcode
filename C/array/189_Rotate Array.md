## 189 Rotate Array
[题目地址](https://leetcode.com/problems/rotate-array/description/)
<br>
<br>
<br>

reverse图解

![](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/array002.jpg)

<br>
<br>

```c
//翻转数组



//方法一：暴力破解法
//每次取出尾结点，剩下依次挪动，最后放到头部，如此进行K-1次
//时间复杂度O（n*k），空间复杂度O（1）

void rotate(int* nums, int numsSize, int k) {
    int i;

    while (k) {
        int tmp=nums[numsSize-1];
        for (i=numsSize-1;i>0;i--) {
            nums[i]=nums[i-1];
        }
        nums[0]=tmp;
        k--;
    }
}



//方法二：使用多余数组
//时间复杂度O（n），空间复杂度O（n）
//具体就不展开



//方法三：使用三次rerverse
//如上图所示
//时间复杂度 O（n）,空间复杂度O（1）

void reverse(int* nums,int start,int end) {
    int temp=0;
    while (start<end) {
        temp=nums[start];
        nums[start]=nums[end];
        nums[end]=temp;

        start++;end--;
    }
}

void three_reverse(int* nums,int numsSize,int n) {
    reverse(nums,0,numsSize-1);
    reverse(nums,0,n-1);
    reverse(nums,n,numsSize-1);
}

void rotate(int* nums,int numsSize,int k) {
    //注意k有可能大于numsSize
    //所以要用k=k%numsSize 取余
    k %=numsSize;
    three_reverse(nums,numsSize,k);
}




//方法四：Using Cyclic Replacements
//利用环的性质进行交换
//不是很理解
//参考discuss:https://leetcode.com/problems/rotate-array/discuss/54282/My-interpretationproof-of-the-Cyclic-Replacements-Method-in-Editorial-Solution

```