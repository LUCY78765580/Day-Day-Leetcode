## 001 Two Sum
[题目地址](https://leetcode.com/problems/two-sum/description/)
<br>
<br>
<br>

```c
/**
* Note: The returned array must be malloced, assume caller calls free().
*/


//方法一：二重循环+flag标志
//借鉴了冒泡排序中加入flag的做法
int* twoSum(int* nums, int numsSize, int target) {
    int *res=(int*)malloc(sizeof(int)*2);
    int i,j;
    int flag;

    for (i=0;i<numsSize;i++) {
        flag=0;
        for (j=i+1;j<numsSize;j++) {
            if (nums[i]+nums[j]==target) {
                res[0]=i;
                res[1]=j;
                flag=1;
                break;
            }
        }
        if (flag==1) break;
    }

    return res;
}


//方法二： 同样二重循环，用goto语句代替flag
int* twoSum(int* nums,int numsSize,int target) {
    int* res=(int*)malloc(sizeof(int)*2);
    int i,j;

    for (i=0;i<numsSize;i++) {
        for (j=i+1;j<numsSize;j++) {
            if (nums[i]+nums[j]==target) {
                res[0]=i;
                res[1]=j;
                goto FOUND;
            }
        }
    }

    FOUND:
        return res;
}


//以上虽然做法简单容易理解,但是复杂度偏高
//方法三：并查集
//参考以下答案：https://leetcode.com/problems/two-sum/discuss/19/Accepted-C-solution-of-HashMap-in-4ms
struct HashNode {
    int key;
    int val;
};

struct HashMap {
    int size;
    struct HashNode** storage;
};

struct HashMap* hash_create(int size) {
    struct HashMap* hashmap=malloc(sizeof(struct HashMap));
    hashmap->size=size;
    hashmap->storage=(struct HashNode**)calloc(size,sizeof(struct HashNode*));
    return hashmap;
}

void hash_destroy(struct HashMap* hashmap) {
    for (int i=0;i<hashmap->size;i++) {
        struct HashNode* node;
        if (node=hashmap->storage[i]) {
            free(node);
        }
    }
    free(hashmap->storage);
    free(hashmap);
}

void hash_set(struct HashMap* hashmap,int key,int value) {
    int hash=abs(key)%hashmap->size;
    struct HashNode* node;

    while (node=hashMap->storage[hash]) {
        if (hash < hashmap->size-1) {
            hash++;
        }else {
            hash=0;
        }
    }
    node=(struct HashNode*)malloc(sizeof(struct HashNode));
    node->key=key;
    node->value=value;
    hashmap->storage[hash]=node;
}

struct HashNode* hash_get(struct HashMap* hashmap,int key) {
    int hash=abs(key)%hashmap->size;
    struct HashNode* node;

    while (node=hashmap->storage[hash]) {
        if (node->key==key) return node;
        if (hash < hashmap->size-1) {
            hash++;
        }
        else {
            hash=0;
        }
    }

    return NULL;
}

int* twoSum(int* nums,int numsSize,int target) {
    struct HashMap* hashmap;
    struct HashNode* node;
    int rest,i;

    //hashmap的大小是2倍的numsSize
    hashmap=hash_create(numsSize*2);
    for (i=0;i<numsSize;i++) {
        rest=target-nums[i];
        node=hash_get(hashmap,res);
        if (node) {
            int* res=(int*)malloc(2*sizeof(int));
            res[0]=node->val+1;
            res[1]=i+1;
            hash_destroy(hashmap);
            return res;
        }
        else {
            hash_set(hashmap,nums[i],i);
        }
    }
}

//方法四：快速排序+二分查找
//参考以下答案：https://leetcode.com/problems/two-sum/discuss/721/My-C-6ms-solution-using-QuickSort-and-binarySearch
```