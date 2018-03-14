## 215 Kth Largest Element in an Array
[题目地址](https://leetcode.com/problems/kth-largest-element-in-an-array/description/)
<br>
<br>
<br>

```c
//查找数组中第K大的元素
//可以考虑先排序，排序又分很多种了


//冒泡排序
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



//快速排序
void QSort(int* nums,int left,int right) {
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
    QSort(nums,left,i-1);
    QSort(nums,i+1,right);
}

void Quick_Sort(int* nums,int numsSize) {
    QSort(nums,0,numsSize-1);
}



//直接插入排序
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



//希尔排序
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

int findMin(int* nums,int numsSize,int x) {
    int j,k;

    k=x;
    for (j=x+1;j<numsSize;j++) {
        k=(nums[k]<nums[j])?k:j;
    }
    return k;
}



//直接选择排序
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





//堆排序
void PercDown(int* H,int x,int len) {
    int parent,child;

    int temp=H[x];
    for (parent=x;(parent*2+1)<= (len-1);parent=child) {
        child=parent*2+1;
        if (child<len-1&&H[child]<H[child+1])
            child++;
        if (temp>H[child]) break;
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




//归并排序
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

void MSort(int* A,int* TmpA,int left,int right) {
    if (left<right) {
        int mid=(left+right)/2;

        MSort(A,TmpA,left,mid);
        MSort(A,TmpA,mid+1,right);
        Merge(A,TmpA,left,mid+1,right);
        //注意这里是mid+1(左边起始点、右边起始点、右边终点)
    }
}

void Merge_Sort(int* nums,int numsSize) {
    int* numsA=(int*)malloc(numsSize*sizeof(int));

    if (numsA!=NULL) {
        MSort(nums,numsA,0,numsSize-1);
        free(numsA);
    }
}





//桶排序
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


int findKthLargest(int* nums, int numsSize, int k) {
    //这里换成 Bubble_Sort/Insert_Sort/Shell_Sort/Select_Sort/Heap_Sort/Merge_Sort/Bucket_Sort
    //桶排序还是不会233   Line 256: index -1 out of bounds for type 'int [10][20]'
    //桶排序这里不适合负数的情况
    Quick_Sort(nums,numsSize);

    int* p=nums+(numsSize-1);

    for (int i=k;k>1;k--) {
        p--;
    }
    return *p;
}

```