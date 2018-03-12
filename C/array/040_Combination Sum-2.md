## 040  Combination Sum II
[题目地址](https://leetcode.com/problems/combination-sum-ii/description/)
<br>
<br>
<br>

```c
/**
 * Return an array of arrays of size *returnSize.
 * The sizes of the arrays are returned as *columnSizes array.
 * Note: Both returned array and *columnSizes array must be malloced, assume caller calls free().
 */

//这一题是 039  Combination Sum 的变式
//这时候，数组中的元素 不能 多次使用，每一个元素只能用一次


//只需要改一下细节即可
//1）将i变成i+1
//2）添加一个判断函数，判断solution是否已经存在
//3）当然中间需要cache1/cache2辅助数组

int* cache1,cache2;

void Insert_Sort(int* nums,int size) {
    int i,j;

    for (i=1;i<size;i++) {
        int temp=nums[i];
        for (j=i-1;j>=0&&temp<nums[j];j--) {
            nums[j+1]=nums[j];
        }
        nums[j+1]=temp;
    }
}

//count是solution数组的长度
int exist(int** results,int len, int* solution,int* column_sizes,int count) {
    int i, j;

    //对整个column_sizes大数组遍历，找出len恰好是columns_sizes[i]的其中的小数组results[i]
    for (i = 0; i < count; i++) {

        //当前长度和solution长度一样，才判断是否是相同
        //分别拷贝到cache1和cache2，排序然后比较
        if (len == column_sizes[i]) {
            memcpy(cache1, results[i], len * sizeof(int));
            memcpy(cache2, solution, len * sizeof(int));

            Insert_Sort(cache1, len);
            Insert_Sort(cache2, len);

            if (memcmp(cache1, cache2, len * sizeof(int))==0) {
                return 1;
            }
        }
    }

    return 0;
}


void combination_recursive(int* nums,int numsSize,int start,int len,int target,int* solution,int** result,int* column_sizes,int* count) {
    int i;
    if (target>0) {
        for (i=start;i<numsSize;i++) {
            solution[len++]=nums[i];

            //将i,改为i+1即可
            //当前与前面，不能重复之意（一个数不能取2次）
            combination_recursive(nums,numsSize,i+1,len,target-nums[i],solution,result,column_sizes,count);
            len--;
        }

    }
    else if (target==0) {
        //这里多加一句判断
        if (!exist(result,len,solution,column_sizes,*count)) {
        result[*count]=(int*)malloc(len*sizeof(int));
        memcpy(result[*count],solution,len*sizeof(int));
        column_sizes[*count]=len;
        (*count)++;
        }
    }
}

//主函数
int** combinationSum2(int* candidates, int candidatesSize, int target, int** columnSizes, int* returnSize) {
    Insert_Sort(candidates,candidatesSize);

    int total=1000;
    int** res=(int**)malloc(total*sizeof(int*));
    *columnSizes=(int*)malloc(total*sizeof(int));


    int* solution=(int*)malloc(target*sizeof(int));
    cache1=(int*)malloc(target*sizeof(int));
    cache2=(int*)malloc(target*sizeof(int));

    *returnSize=0;
    combination_recursive(candidates,candidatesSize,0,0,target,solution,res,*columnSizes,returnSize);
    return res;
}
```