## 027 Remove Element
[题目地址](https://leetcode.com/problems/remove-element/description/)
<br>
<br>
<br>


```c
//方法一：双重循环（算是比较蠢的方法了）
int removeElement(int* nums,int numsSize,int val) {
    int i,j;
    int *p=nums;

    //修改数组
    //注意这里的i--,是为了回到原位
    for (i=0;i<numSize;i++) {
        if (nums[i]==val) {
            for (j=i+1;j<numsSize;j++) {
                nums[j-1]=nums[j];
            }
            i--;
        }
    }

    //求数组长度
    int len=0;
    while (*p) {
        len++;
        p++;
    }

    return len;
}


//方法二：和026很相似（这里理解要简单些）
//注意这里的Do not allocate extra space for another array
//modifying the input array in-place with O(1) extra memory

int removeElement(int* nums,int numsSize,int val) {
    int i;
    int count=0;

    for (i=0;i<numsSize;i++) {
        if (nums[i]!=val) {
            nums[count++]=val;
        }
    }

    return count;
}
```