## 016  3Sum Closest
[题目地址](https://leetcode.com/problems/3sum-closest/description/)
<br>
<br>
<br>

```c
//最接近的三数之和

//方法一：排序后查找

void Bubble_Sort(int* nums,int numsSize) {
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

int threeSumClosest(int* nums, int numsSize, int target) {
    if (!nums||numsSize<3) return NULL;

    int i,j,k;
    int res,sum;

    Bubble_Sort(nums,numsSize);

    res=nums[0]+nums[1]+nums[2];
    for (i=0;i<numsSize-2;i++) {
        j=i+1;
        k=numsSize-1;

        while (j<k) {
            sum=nums[i]+nums[j]+nums[k];
            res=(abs(res-target)<abs(sum-target))?res:sum;

            if (sum<target) j++;
            else k--;
        }
    }
    return res;
}





//方法二：并查集


```