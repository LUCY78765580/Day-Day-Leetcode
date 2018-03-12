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

//审题
//大数组长度为*returnSize,其中小数组长度存在*columnSizes数组当中
//理解错题目意思？[1,2,3] 中[1,3]也是算数的，老套路是行不通的了 只能用递归咯

//函数：void* memcpy ( void * dest, const void * src, size_t num );
//memcpy 会复制src所指的内存内容的前num个字节到dest所指的内存地址上
//参考博客  http://c.biancheng.net/cpp/html/155.html


//函数：int memcmp (const void *a1, const void *a2, size_t size)
//memcmp用于比较字符串s1与s2, 的前size个字符,如果两上字符块相同，memcmp将返回0
//参考博客  http://c.biancheng.net/cpp/html/154.html

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

//判断子序列是否在大数组中已经存在
//buf是当前小数组（尚未存入大数组中）
int exist(int** results, int count, int* sizes, int* buf, int len)
{
    int i, j;
    for (i = 0; i < count; i++) {
        //如果len恰好等于res中某一个小数组（res[count]）的长度
        if (len == sizes[i]) {
            //将此时的res[i]和buf,前面len长度
            //分别拷贝到cache1和cache2中
            memcpy(cache1, results[i], len * sizeof(int));
            memcpy(cache2, buf, len * sizeof(int));

            //分别排序
            Bubble_Sort(cache1, len);
            Bubble_Sort(cache2, len);

            //比较前面len长度
            if (memcmp(cache1, cache2, len * sizeof(int))==0) {
                return 1;
            }
        }
    }
    return 0;
}

//递归函数

//size对应returnSize,原数组nums的长度
//*count对应returnSize，大数组的长度
//*sizes对应*columnSizes,小数组长度的总数组
//start和len，len对应某一个res[*count]小数组的长度
//buf对应，未存入res[*count]中的小数组

void subset_recursive(int *nums, int size, int start,int *buf, int len,int **res, int *sizes, int *count) {
    int i;
    //如果没有存在则加入
    //最开始len=0,start=0,所以存入一个[]
    if (!exist(res, *count, sizes, buf, len)) {
        res[*count] = malloc(len * sizeof(int));
        memcpy(res[*count], buf, len * sizeof(int)); //将buf中前面len个数组长度拷贝到res[*count]中
        sizes[*count] = len;
        (*count)++;
    }

    //这一段不是很懂了
    for (i = start; i < size; i++) {
        buf[len++] = nums[i];
        subset_recursive(nums, size, i + 1, buf, len, res, sizes, count);
        len--;
    }
}


//总函数
int** subsetsWithDup(int* nums, int numsSize, int** columnSizes, int* returnSize) {
    int total=5000;
    int** res = (int**)malloc(total*sizeof(int*));
    *columnSizes = (int*)malloc(total*sizeof(int));
    int* buf = (int*)malloc(numsSize*sizeof(int));

    cache1 = (int*)malloc(numsSize*sizeof(int));
    cache2 = (int*)malloc(numsSize*sizeof(int));
    *returnSize = 0;

    subset_recursive(nums,numsSize,0,buf,0,res,*columnSizes,returnSize);
    return res;
}
```