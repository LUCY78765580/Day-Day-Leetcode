## 039  Combination Sum
[题目地址](https://leetcode.com/problems/combination-sum/description/)
<br>
<br>
<br>

```c
/**
 * Return an array of arrays of size *returnSize.
 * The sizes of the arrays are returned as *columnSizes array.
 * Note: Both returned array and *columnSizes array must be malloced, assume caller calls free().
 */

//寻找和为指定值的 子数组集合
//注意：数组中的元素可以是重复的


//方法一：排序+回溯（backtracking）（或者递归）
//一些使用回溯的题型汇总，见discuss:https://leetcode.com/problems/combination-sum/discuss/16502/A-general-approach-to-backtracking-questions-in-Java-(Subsets-Permutations-Combination-Sum-Palindrome-Partitioning)

void Insert_Sort(int* nums,int size) {
    int i,j;

    for (i=1;i<size;i++) {
        int temp=nums[i];
        for (j=i-1;i>0&&temp<nums[j];j--) {
            nums[j+1]=nums[j];
        }
        nums[j+1]=temp;
    }
}


void combination_recursive(int* nums,int numsSize,int start,int len,int target,int* solution,int** result,int* column_sizes,int* count) {
    int i;
    if (target>0) {
        for (i=start;i<numsSize;i++) {

            //这一句最难理解了，先len++,递归,再len--
            //每深入递归一层，len长度加一，i为下一层的start值
            solution[len++]=nums[i];
            combination_recursive(nums,numsSize,i,len,target-nums[i],solution,result,column_sizes,count);
            len--;
        }

    }
    //target==0时，此时获取一个solution小数组
    //len为solution的长度
    else if (target==0) {
        result[*count]=(int*)malloc(len*sizeof(int));
        memcpy(result[*count],solution,len*sizeof(int));
        column_sizes[*count]=len;
        (*count)++;
    }
}

int** combinationSum(int* candidates, int candidatesSize, int target, int** columnSizes, int* returnSize) {
    Insert_Sort(candidates,candidatesSize);

    int total=1000;
    int** res=(int**)malloc(total*sizeof(int*));
    *columnSizes=(int*)malloc(total*sizeof(int));
    int* solution=(int*)malloc(target*sizeof(int));
    //注意这里是target
    //因为最多占用target个空间(每一个都是１)

    *returnSize=0;
    combination_recursive(candidates,candidatesSize,0,0,target,solution,res,*columnSizes,returnSize);

    return res;
}



//方法二：动态规划
//参考discuss:https://leetcode.com/problems/combination-sum/discuss/16549/Short-easy-16-lines-bottom-up-DP-C++-solution-(15ms)

```