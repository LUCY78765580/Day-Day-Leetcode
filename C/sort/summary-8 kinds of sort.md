### 八大排序
<br>
<br>

### 概述

排序分**内部排序**和**外部排序**。内部排序是数据记录在内存中进行排序，外部排序需要访问外存（数据量很大）
一般八大排序指的是内部排序。各种排序及其复杂度如下图所示。

![sort001](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/sort001.jpg)

![sort002](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/sort002.jpg)

以下举Leetcdoe中 [15、3sum](https://leetcode.com/problems/3sum/description/) 这一题为例:blush:
<br>
<br>
<br>

### 1、冒泡排序(Bubble Sort)

原理：

待排序n个数中，自上而下对相邻2个数进行比较调整。使较大的往下沉，小的往上冒（交换）。
排完一趟后最大的已沉到最底部。对剩下n-1个数同理，直到所有数据完全按顺序排列。
冒泡排序中，常常加入变量flag，用以标志在某一趟中是否有数据交换。如果没有数据交换，
则说明数据已完全按顺序排列，这时便可退出循环结束整个排序。

图解：

![sort003](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/sort003.jpg)

算法：
```c
void Bubble_Sort(int* nums,int numsSize) {
    int p,i;
    int temp;
    int flag=0;

    for (p=numsSize-1;p>=0;p--) {
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
```
<br>
<br>
<br>

### 2、快速排序(Quick Sort)

原理：
- 选主元：选择基准元素（一般取第一个或最后一个）
- 子集划分：一趟排序将数据分成2个部分，一边所有值皆比主元小，一边所有值皆比主元大
- 分而治之：对这2部分同理，用同样方法排序直到整个序列有序

- 子集划分后，主元被一次性放到了最终的正确位置上，再也不改变，这也是快速排序为什么效率高的原因
- 快速排序是通常被认为在同数量级（O(nlog2n)）的排序方法中平均性能最好的。
但若初始序列按关键码有序或基本有序时，快排序反而蜕化为冒泡排序。为改进之，
通常以“三者取中法”来选取基准记录，即将排序区间的两个端点与中点三个记录关键码居中的调整为支点记录。


图解：

![sort004](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/sort004.jpg)

![sort005](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/sort005.jpg)


算法：

```c
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
```
<br>
<br>
<br>

### 3、直接插入排序(Straight Insertion Sort)

原理：
- 插入排序有些类似于打牌，就是在已有数据的基础上，不断比较并插入新的数据
- 关键之处在于确立哨兵，每一趟排序选取待插入数据为哨兵

图解：

![sort006](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/sort006.jpg)

算法：

```c
void Insert_Sort(int* nums,int numsSize) {
    int p,i;
    int temp;

    for (p=1;p<numsSize;p++) {
        temp=nums[p];
        for (i=p;i>0&&temp<nums[i-1];i--) {
            nums[i]=nums[i-1];
        }
        nums[i]=temp;
    }
}
```
<br>
<br>
<br>

### 4、希尔排序(Shell Sort)

原理：

- 逆序对：对于下标i<j,如果A[i]>A[j],则称（i,j）是一对逆序对
- 冒泡排序和选择排序，都是通过每一次交换相邻的两元素，从而消去一个逆序对。
但是想要提高算法效率，就要每次消去不止一个逆序对（即每一次交换相隔较远大于2的两个元素）
- 希尔排序是1959 年由D.L.Shell 提出来的，整体思想：定义一个具体的增量序列，
将原本大序列分割成一个个较小的子序列，分别进行直接插入排序。
- 希尔排序一个重要的性质是，"Dk-间隔"有序序列，在执行完"Dk-1"间隔有序排列后，仍然
可以保持原来的"Dk-间隔"有序
- 希尔排序依赖于增量序列，不同场合下增量序列可能是不一样的，因此希尔排序是不稳定的

图解：

![sort007](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/sort007.jpg)

算法：

```c
//取增量序列为Dk = D(k+1)/2
void Shell_Sort(int* nums,int numsSize) {
    int p,i;
    int temp;
    int D=numsSize/2;

    for (D;D>0;D/=2) {
        for (p=1;p<numsSize;p++) {
            temp=nums[p];
            for (i=p;i>=D&&temp<nums[i-D];i-=D)
                nums[i]=nums[i-D];
            nums[i]=temp;
        }
    }
}
```
<br>
<br>
<br>


### 5、简单选择排序（Simple Selection Sort）

原理：

在待排序列中，选出最大（或最小值）与第1位置交换；剩下的数中，以此类推，一直到完全按顺序排序为止

图解：

![sort008](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/sort008.jpg)

算法：

```c
//找到最小的数，所在的位置
//i从0开始到最后遍历，如果当前位置i不是最小值所在位置，则用temp进行交换
int findMin(int* nums,int numsSize,int x) {
    int j,k;

    k=x;
    for (j=x+1;j<numsSize;j++) {
        k=(nums[k]<nums[j])?k:j;
    }
    return k;
}

void Select_Sort(int* nums,int numsSize) {
    int i,key;
    int temp;

    for (i=0;i<numsSize;i++) {
        key=findMin(nums,numsSize,i);
        if (key!=i) {
            temp=nums[i];nums[i]=nums[key];nums[key]=temp;
        }
    }
}
```
<br>
<br>
<br>

### 6、堆排序(Heap Sort)

原理：

- 简单选择排序操作简单，但是findMin函数查找往往花费大量时间，如何提高效率，
以快速找到最小元（或最大元）的位置，这就运用到了最小堆（or最大堆）。
- 堆排序初始时将n个数序列，看成一个顺序存储的二叉树，并调整顺序使成为一个堆。
将堆顶元素输出得到最大最小值并排列，同时不断调整剩余元素为堆，一直到全部排完
- 主要解决两个问题：1、如何将n个待排元素建立成堆 2、输出堆顶元素后，如何调整剩余元素成为一个完整新堆

- 特别注意: 初始化大顶堆时 是从**最后一个有子节点**开始往上调整最大堆。
而堆顶元素(最大数)与堆最后一个数交换后，**需再次调整成大顶堆，此时是从上往下调整的**。

- 不管是初始大顶堆的从下往上调整，还是堆顶堆尾元素交换，每次调整都是从父节点、
左孩子节点、右孩子节点三者中选择最大者跟父节点进行交换，交换之后，
都可能造成被交换的孩子节点不满足堆的性质，因此每次交换之后要重新对被交换的孩子节点进行调整。


图解：

![sort013](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/sort013.jpg)

![sort009](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/sort009.jpg)

![sort010](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/sort010.jpg)

![sort011](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/sort011.jpg)

![sort012](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/sort012.jpg)

算法：

```c
//建堆，并由上到下过滤，调整为最大堆
//最大堆过滤函数和DeleteMax类似，不同的是，PercDown不从0开始而从x=i开始，且只过滤不返回值
void PercDown(int* H,int x,int len) {
    int parent,child;

    int temp=H[x];  　　#最大堆最后一个元素存储在temp中
    for (parent=x;(parent*2+1)<= (len-1);parent=child) {
        child=parent*2+1;
        if (child<len-1&&H[child]<H[child+1])  #找到左右节点中较大的那一个
            child++;
        if (temp>H[child]) break;              #如果父节点比最大子节点小，则交换，反之退出循环
        else {
            H[parent]=H[child];
        }
    }

    H[parent]=temp;
}

//建堆
void BuildHeap(int* nums,int numsSize) {
    for (int i=(numsSize-1)/2;i>=0;i--) {
        PercDown(nums,i,numsSize);
    }
}

//堆排序
void Heap_Sort(int* nums,int numsSize) {
    //建立最大堆
    BuildHeap(nums,numsSize);

    //交换堆顶和最后一个元素的位置，由上到下开始调整(从0开始)
    //每次交换后，长度要减1，所以i--
    for (int i=numsSize-1;i>=0;i--) {
        int temp=nums[0];nums[0]=nums[i];nums[i]=temp;
        PercDown(nums,0,i);
    }
}
```
<br>
<br>
<br>

### 7、归并排序(Merge Sort)

原理：

- 应用分治+递归思想，待排序序列分为若干个有序子序列，再将子序列合并为整体有序序列
- 归并部分，需要一个临时数组辅助

图解：

![sort014](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/sort014.jpg)

算法：
```c
//定义归并函数
//L、LeftEnd、R、RightEnd分别代表左边起点、左边终点、右边起点、右边终点
//A,TmpA分别为数组、临时数组，len为数组长度
void Merge(int* A,int* TmpA,int L,int R,int RightEnd) {
    int LeftEnd=R-1;
    int Tmp=L;
    int len=RightEnd-L+1;

    while (L<=LeftEnd&&R<=RightEnd) {
        if (A[L]<A[R]) TmpA[Tmp++]=A[L++];
        else TmpA[Tmp++]=A[R++];
    }

    while (L<=LeftEnd) TmpA[Tmp++]=A[L++];
    while (R<=RightEnd) TmpA[Tmp++]=A[R++];

    //最后从结尾，将临时数组TmpA中数据倒入A中
    for (int i=0;i<len;i++,RightEnd--) {
        A[RightEnd]=TmpA[RightEnd];
    }
}

//定义Msort函数（分而治之+递归）
void MSort(int* A,int* TmpA,int left,int right) {
    if (left<right) {
        int mid=(left+right)/2;

        MSort(A,TmpA,left,mid);
        MSort(A,TmpA,mid+1,right);
        Merge(A,TmpA,left,mid+1,right);
        //注意这里是mid+1(左边起始点、右边起始点、右边终点)
    }
}

//统一接口
void Merge_Sort(int* nums,int numsSize) {
    int* numsA=(int*)malloc(numsSize*sizeof(int));

    if (numsA!=NULL) {
        MSort(nums,numsA,0,numsSize-1);
        free(numsA);
    }
}
```
<br>
<br>
<br>

### 8、基数排序(Radix Sort)

原理：

- 桶排序（Bucket Sort）:简单来说就是把数据分组，放在有限数量的桶中，然后对每个桶里的数据再进行排序。当要被排序的阵列内的数值是均匀分配的时候，桶排序使用线性时间（Θ（n））。

 例如要对大小为[1..1000]范围内的n个整数A[1..n]排序

 首先，可以把桶设为大小为10的范围，具体而言，设集合B[1]存储[1..10]的整数，
 集合B[2]存储(10..20]的整数，……集合B[i]存储(   (i-1)*10,   i*10]的整数，i=1,2,..100。总共有100个桶。

 然后，对A[1..n]从头到尾扫描一遍，把每个A[i]放入对应的桶B[j]中。再对每个桶里的数字排序，
 这时可用冒泡，选择，乃至快排等任何排序法。

 最后，依次输出每个桶里面的数字，且每个桶中的数字从小到大输出，这  样就得到所有数字排好序的一个序列了。

- 桶排序能实现接近O（n）的时间复杂度，但是也有相应的缺点：首先是空间复杂度比较高，
需要的额外开销大，其次待排序的元素都要在一定的范围内等等。

- 与桶排序类似，基数排序通过“分配”和“收集”过程来实现排序，无须比较关键字，时间复杂度亦可达到线性阶：O(n)

扑克牌中52 张牌，可按花色和面值分成两个字段，其大小关系为：
花色： 梅花< 方块< 红心< 黑心
面值： 2 < 3 < 4 < 5 < 6 < 7 < 8 < 9 < 10 < J < Q < K < A

- 最高位优先(Most Significant Digit first)法，简称MSD法：先对花色排序并将其分为4组，每个组分别按面值排序，最后将4组连接。
- 最低位优先(Least Significant Digit first)法，简称LSD法：先按13个面值分成13 堆，再按花色给出4 个编号组，将2号组中牌取出分别放入对应花色组，再将3 号组中牌取出分别放入对应花色组，……，这样4 个花色组中均按面值有序，然后将4 个花色组依次连接起来即可。

<br>
图解：

![sort015](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/sort015.jpg)


算法
```c
void Bucket_Sort(int* nums, int numsSize)
{
    //获取数组中的最大数
    int maxNum = findMaxNum(nums, numsSize);

    //获取最大数的位数
    int loopTimes = getLoopTimes(maxNum);
    int i;

    //对每一位进行桶分配
    for (i = 1; i <= loopTimes; i++) {
        sort2(nums, numsSize, i);
    }
}


//获取数字的位数
int getLoopTimes(int num) {
    int count = 1;
    int temp = num / 10;

    while (temp != 0) {
        count++;
        temp = temp / 10;
    }
    return count;
}

//查询数组的最大数
int findMaxNum(int* nums, int numsSize) {
    int max = INT_MIN;

    for(int i = 0; i < numsSize; i++) {
        max=(*(nums+i) > max)? *(nums+i):max;
    }
    return max;
}

//将数字分配到各自的桶中，然后按照桶的顺序输出排序结果
void sort2(int* nums, int numsSize, int loop) {
    //建立一组桶此处的20是预设的根据实际数情况修改
    int buckets[10][20] = {};
    //求桶的index的除数
    //如798个位桶index=(798/1)%10=8
    //十位桶index=(798/10)%10=9
    //百位桶index=(798/100)%10=7
    //tempNum为上式中的1、10、100
    int tempNum = (int)pow(10, loop - 1);
    int i, j;

    for (i = 0; i < numsSize; i++) {
        int row_index = (*(nums + i) / tempNum) % 10;
        for (j = 0; j < 20; j++) {
            if (buckets[row_index][j] == NULL) {
                buckets[row_index][j] = *(nums+ i);
                break;
            }
        }
    }

    //将桶中的数，倒回到原有数组中
    int k = 0;
    for (i = 0; i < 10; i++) {
        for(j = 0; j < 20; j++) {
            if(buckets[i][j] != NULL) {
                *(nums + k) = buckets[i][j];
                buckets[i][j] = NULL;
                k++;
            }
        }
    }
}
```

<br>
<br>
<br>


参考博客：
1、http://blog.csdn.net/hguisu/article/details/7776068

2、https://www.cnblogs.com/0zcl/p/6737944.html
