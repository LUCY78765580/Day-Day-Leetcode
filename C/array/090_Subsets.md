## 090  Subsets II
[题目地址](https://leetcode.com/problems/subsets-ii/description/)
<br>
<br>
<br>

```c
/**
 * Return an array of arrays of size *returnSize.
 * The sizes of the arrays are returned as *columnSizes array.
 * Note: Both returned array and *columnSizes array must be malloced, assume caller calls free().
 */

//求序列的所有子列
//这一题和之前的 039 Combination Sum/040 Combination Sum II异曲同工
//都可以用回溯算法



/*
1)函数：void* memcpy ( void * dest, const void * src, size_t num );
memcpy 会复制src所指的内存内容的前num个字节到dest所指的内存地址上
参考博客  http://c.biancheng.net/cpp/html/155.html

2)函数：int memcmp (const void *a1, const void *a2, size_t size)
memcmp用于比较字符串s1与s2, 的前size个字符,如果两上字符块相同，memcmp将返回0
参考博客  http://c.biancheng.net/cpp/html/154.html
*/


int *cache1, *cache2;

void Bubble_Sort(int* nums,int numsSize) {
    int p,i;
    int flag,temp;
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


int exist(int** res,int count,int* column_sizes,int* solution,int len) {
    int i,j;

    for (i=0;i<count;i++) {
        if (len==column_sizes[i]) {
            memcpy(cache1,res[i],len*sizeof(int));
            memcpy(cache2,solution,len*sizeof(int));

            Bubble_Sort(cache1,len);
            Bubble_Sort(cache2,len);

            if (memcmp(cache1,cache2,len*sizeof(int))==0) {
                return 1;
            }
        }
    }

    return 0;
}

void subset_recursive(int* nums,int size,int start,int len,int* solution,int** res,int* column_sizes,int* count) {
    int i;

    if (!exist(res,*count,column_sizes,solution,len)) {
        res[*count]=malloc(len*sizeof(int));
        memcpy(res[*count],solution,len*sizeof(int));
        column_sizes[*count]=len;
        (*count)++;
    }

    for (i=start;i<size;i++) {
        solution[len++]=nums[i];
        subset_recursive(nums,size,i+1,len,solution,res,column_sizes,count);
        len--;
    }
}

int** subsetsWithDup(int* nums,int numsSize,int** columnSizes,int* returnSize) {
    int total=5000;
    int** res=(int**)malloc(total*sizeof(int*));
    *columnSizes=(int*)malloc(total*sizeof(int));
    int* solution=(int*)malloc(numsSize*sizeof(int));

    cache1=(int*)malloc(numsSize*sizeof(int));
    cache2=(int*)malloc(numsSize*sizeof(int));
    *returnSize=0;

    subset_recursive(nums,numsSize,0,0,solution,res,*columnSizes,returnSize);
    return res;
}
```