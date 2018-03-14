## 042  Trapping Rain Water
[题目地址](https://leetcode.com/problems/trapping-rain-water/description/)
<br>
<br>
<br>

```c
//接雨水

//方法与前面 011  Container With Most Water相似
//不同的地方：1、本题要求出所有接水总量，前面仅需求最大值   2、本题边缘是不包括在内，前者边缘包括在内


//方法一
//方法二的简写版本
int trap(int* height,int heightSize) {
    int* left=height;int* right=left+heightSize-1;
    int level=0,water=0;

    while (left<right) {
        int lower=*left<*right?*left++:*right--;
        level=level>lower? level:lower;

        water+=level-lower;
    }

    return water;
}


//方法二(本质上是一样的)
int trap(int* height, int heightSize) {
    int left=0;int right=heightSize-1;
    int maxLeft=0;int maxRight=0;
    int water=0;

    while (left<=right) {
        if (height[left]<height[right]) {
            if (height[left]>maxLeft) maxLeft=height[left];
            else water+=maxLeft-height[left];
            left++;
        }
        else {
            if (height[right]>maxRight) maxRight=height[right];
            else water+=maxRight-height[right];
            right--;
        }
    }

    return water;
}
```