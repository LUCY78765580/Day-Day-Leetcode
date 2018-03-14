## 011  Container With Most Water
[题目地址](https://leetcode.com/problems/container-with-most-water/description/)
<br>
<br>
<br>

```c
//最大水容器

//原理：对于任意容器，其容积取决于最短板
//用双指针扫描数组，只移动短板指针往中间搜素，并记录最大值（因为容积取决于最短板，只有最短板发生改变时，才有可能发生最大值的改变）\
//具体：找出左右两边缘较小的那一个，然后乘以两边缘的距离(求最大面积)

//与42 Trapping Rain Water“接雨水”那一题类似，不同的地方：
//1、“接雨水”要求出所有接水总量，这一题仅需求最大值 2、前者边缘不包括在内，本题边缘是包括在内的


//方法一
int maxArea(int* height, int heightSize) {
    //边界条件
    if (!height||heightSize<2) return 0;

    int *i=height;
    int *j=height+(heightSize-1);
    int water=0;

    while (i<j) {
        //容器的高度取决于i,j中比较小的那一个
        int h=(*i<*j)?*i:*j;

        water=(water > ((j-i)*h))?water:(j-i)*h;
        while (i<j&& *i<=h) i++;
        while (i<j&& *j<=h) j--;
    }
    return water;
}



//方法二（本质上是一样的）

int maxArea(int* height, int heightSize) {
    int i=0;int j=heightSize-1;
    int subMax=0;int Max=0;

    while (i<j) {
        //i这边高度比较小，h0=height[i]
        if (height[i]<height[j]) {
            subMax=(j-i)*height[i];
            i++;
        }
        //j这边高度比较小,h0=height[j]
        else {
            subMax=(j-i)*height[j];
            j--;
        }
        Max=(Max>subMax)?Max:subMax;
    }
    return Max;
}
```