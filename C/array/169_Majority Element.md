## 169 Majority Element
[题目地址](https://leetcode.com/problems/majority-element/description/)
<br>
<br>
<br>




```c
//查找多数元素
//这个题可以有多种解法


//方法一：Brute Force（暴力破解,会超时）
//时间复杂度O(n^2),空间复杂度O(1)


//方法二：HashMap并查集
//时间复杂度O(n),空间复杂度O(n)
//参考dicuss:https://leetcode.com/problems/majority-element/discuss/51729/8ms-C-solution-using-hash-table

#define MIN_VALUE 0x80000000
typedef struct _Node {
    int val;
    int count;
} Node;

int majorityElement(int* nums, int numsSize) {
    const int length = numsSize * 2;
    Node hashtable[length];
    int result = 0;

    for(int i = 0; i < length; ++i) {
        hashtable[i].val = MIN_VALUE;
        hashtable[i].count = 0;
    }

    for(int i = 0; i < numsSize; ++i) {
        int position = *(nums + i) % length;
        position += (position >= 0 ? 0 : length);
        while(hashtable[position].val != MIN_VALUE) {
            if(hashtable[position].val == *(nums + i)) {
                hashtable[position].count++;
                break;
            } else {
                position = (position + 13) % length;
            }
        }
        if(hashtable[position].val == MIN_VALUE) {
            hashtable[position].val = *(nums + i);
            hashtable[position].count = 1;
        }
    }
    for(int i = 0; i < length; ++i) {
        if(hashtable[i].count > numsSize >> 1) {
            result = hashtable[i].val;
        }
    }
    return result;
}



//方法三：快速排序
//时间复杂度O(nlgn)，空间复杂度O(1)或者O(n)
//自定义一个排序函数，或者直接用C语言的qsort函数
//qsort函数参考博客：http://blog.csdn.net/zhaozicang/article/details/24174965

void Quick_Sort(int* nums,int left,int right) {
    if (left>=right) return ;

    int i=left;
    int j=right;
    int key=nums[i];

    while (i<j) {
        while (i<j&&nums[j]>key) j--;
        if (i<j) {
            nums[i++]=nums[j];
        }

        while (i<j&&nums[i]<key) i++;
        if (i<j) {
            nums[j--]=nums[i];
        }
    }

    nums[i]=key;
    Quick_Sort(nums,left,i-1);
    Quick_Sort(nums,i+1,right);
}

int cmp(const void* a,const void* b) {
   return  *(int*)b - *(int*)a;
}

int majorityElement(int* nums,int numsSize) {
    Quick_Sort(nums,0,numsSize-1);
    //qsort(nums,numsSize,sizeof(int),cmp);
    return nums[numsSize/2];
}




//方法四：分治+递归
//时间复杂度O（nlog(n)）,空间复杂度O（lg(n)）--->占用系统堆栈
//参考：https://leetcode.com/problems/majority-element/discuss/51822/Divide-and-Conquer-solution-to-majority-element-problem-(not-optimal-O(nlogn))


//写不出啊啊啊啊
//下面这个是错的
int majority(int* nums,int start,int end) {
    if (start==end) return nums[start];

    int mid=(start+end)/2;
    int left=majority(nums,start,mid);
    int right=majority(nums,mid+1,end);
    if (left==right) return left;

    int leftCount=0;
    int rightCount=0;
    for (int i=start+1;i<end;i++) {
        if (nums[i]==left) {
            leftCount++;
        }
        if (nums[i]==right) {
            rightCount++;
        }
    }

    return leftCount>rightCount?leftCount:rightCount;
}

int majorityElement(int* nums, int numsSize) {
    return majority(nums,0,numsSize-1);
}




//方法五：巧妙的Moore 算法(Boyer-Moore Voting Algorithm)
//时间复杂度O(n)，空间复杂度O(1)

/*
1）极限的情况是：由题意，每两个数中至少有一个是target数字
2）所以如果当前nums[i]==major，计数器加1，不等于则减1
3）如果该数就是majority，那么一直到最后count必然是大于0的，major可能发生变化，最后直接返回major即可
4）也就是说，如果count==0时，重新选major
*/

int majorityElement(int* nums, int numsSize) {
    int count=0;
    int major;

    for (int i=0;i<numsSize;i++) {
        //如果count为0,说明当前没有候选元素
        if (count==0) {
            major=nums[i];
            count++;
        }
        else count+=(nums[i]==major)?1:-1;

        //可以表达式简写
        //else if (nums[i]==major) count++;
        //else count--;
    }

    //如果在这个过程中，count一直保持不为0
    //那么major将是不变的，最后返回major即可
    return major;
}




//方法六：Random算法（Random-agorithms）
/*
以下是java版本代码
public class Solution {
    public int majorityElement(int[] nums) {
        for (int i=0;i<5;i++)
        {
            int v1=((int)(Math.random()*nums.length*10))%nums.length;
            if(isMajor(nums,nums[v1]))
                return  nums[v1];
        }
        return  0;

    }
    public  boolean isMajor(int[] nums,int v)
    {
        int n=0;
        for (int i=0;i<nums.length;i++)
        {
            if(nums[i]==v)
                n++;
        }
        return  n>nums.length/2;
        }

}
*/



//方法七：比特位操作
//这里参考dicuss:https://leetcode.com/problems/majority-element/discuss/51852/C-different-solutions(Array-sort-bit-manipulation(2-kinds)-moore's-voting-algorithm-hash-table)
//看不懂的

int majorityElement(int* nums, int numsSize) {
    int result = 0;
    int bit[32] = {};
    for(int i = 0; i < numsSize; ++i) {
        for(int j = 0; j < 32; ++j) {
            if((*(nums + i) & (1 << j)) != 0) {
                ++bit[j];
            }
        }
    }

    for(int i = 0; i < 32; ++i) {
        if(bit[i] > numsSize >> 1) {
            result |= (1 << i);
        }
    }

    return result;
}
```
