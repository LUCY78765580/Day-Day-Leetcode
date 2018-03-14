## 034 Search for a Range
[题目地址](https://leetcode.com/problems/search-for-a-range/description/)
<br>
<br>
<br>

```c
/**
 * Return an array of size *returnSize.
 * Note: The returned array must be malloced, assume caller calls free().
 */

//查找搜索区间


//数组升序排列，找某一个target的起始位置、终点位置，若找不到则返回[-1,-1](可以直接初始化，找到再改就是咯)
//复杂度规定为O(log(n)),  （看到这，立马想到用二分法了）
/*
如果target>nums[mid]，那么begin应该在mid右边，即start=mid+1
如果target<nums[mid],那么begin应该在mid左边（其实end也应该在左边）,即end=mid-1
如果target==nums[mid] 那么begin可能是mid,也可能在mid左边
*/

//方法一：O(n)的复杂度(从复杂度来说是不满足题意的)
int* searchRange(int* nums, int numsSize, int target, int* returnSize) {
    int* res=(int*)malloc(2*sizeof(int));
    *returnSize=2;
    res[0]=res[1]=-1;
    int i,j;

    if (nums&&numsSize!=0) {
        for (i=0;i<numsSize;i++) {
            if (target==nums[i]) {
                res[0]=i;
                break;
            }
        }
        for (j=numsSize-1;j>=0;j--) {
            if (target==nums[j]) {
                res[1]=j;
                break;
            }
        }
    }

    return res;
}



//方法二：二分法，O(log(n))的复杂度
//思路和上面类似,但是仍然有冗赘
int findFirst(int* nums,int numsSize,int target) {
    int start=0;int end=numsSize-1;
    int flag=-1;

    while (start<=end) {
        int mid=(start+end)/2;
        if (target==nums[mid]) flag=mid;

        //target在左边
        //注意这里的=号是不能省略的
        //例如[1,3,9,9,9,10,12,15]
        //首先第一步就是要往左边走
        if (target<=nums[mid]) {
            end=mid-1;
        }
        else {
            start=mid+1;
        }

    }
    return flag;
}

int findLast(int* nums,int numsSize,int target) {
    int start=0;int end=numsSize-1;
    int flag=-1;

    while (start<=end) {
        int mid=(start+end)/2;
        if (target==nums[mid]) flag=mid;

        //同样注意这里的=号是不能省略的（同上）
        //首先第一步就是要往右边走
        if (target>=nums[mid]) {
            start=mid+1;
        }
        else {
            end=mid-1;
        }

    }
    return flag;
}

int* searchRange(int* nums, int numsSize, int target, int* returnSize) {
    int* res=(int*)malloc(sizeof(int)*2);
    *returnSize=2;

    res[0]=findFirst(nums,numsSize,target);
    res[1]=findLast(nums,numsSize,target);
    return res;
}



//方法三：简化递归版二分法
//上面的二分法虽然容易理解，但是每次查找begin位置后，还要重新开始查找end，其实是不必要的，递归可以省略一些

void search(int* nums,int target,int start,int end,int* res) {
    if (start>end) return NULL;
    //这个(start+end)/2 和 start+(end-start)/2是一样的效果
    int mid=(start+end)/2;

    //target等于nums[mid]
    //最开始res[0]为numsSize
    //最开始res[1]为-1
    //接着res[0]往左边递归，res[1]往右边递归
    if (target==nums[mid]) {
        if (mid<res[0]) {
            res[0]=mid;
            search(nums,target,start,mid-1,res);
        }
        if (mid>res[1]) {
            res[1]=mid;
            search(nums,target,mid+1,end,res);
        }
    }
    //target小于nums[mid]，往左边递归
    else if (target<nums[mid]) {
        search(nums,target,start,mid-1,res);
    }
    //taget大于nums[mid]，往后边递归
    else {
        search(nums,target,mid+1,end,res);
    }
}

int* searchRange(int* nums, int numsSize, int target, int* returnSize) {
    int* res=(int*)malloc(sizeof(int)*2);
    *returnSize=2;
    res[0]=numsSize;
    res[1]=-1;

    search(nums,target,0,numsSize-1,res);

    //这一句是不能去掉的
    //即res[0]没有变仍为之前的numsSize
    //这时要将其变成-1
    if (res[0]>res[1]) {
        res[0]=-1;
    }

    return res;
}
```