## 023 Merge K Sorted Lists
[题目地址](https://leetcode.com/problems/merge-k-sorted-lists/description/)
<br>
<br>
<br>

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */

//方法一：分治后相连(分治+递归/迭代)
//首先mergeTwoLists函数,其实就是前面未变形那一题
struct ListNode *mergeTwoLists(struct ListNode *l1, struct ListNode *l2) {
    if (l1==NULL&&l2==NULL) return NULL;
    if(l1==NULL) return l2;
    if(l2==NULL) return l1;

    if(l1->val<=l2->val){
        l1->next=mergeTwoLists(l1->next,l2);
        return l1;
    }
    else{
        l2->next=mergeTwoLists(l1, l2->next);
        return l2;
    }
}

//1)迭代形式
struct ListNode* mergeKLists(struct ListNode** lists,int listsSize) {
    int n=listsSize;

    //必须要两个或者以上才能归并
    //两个两个归并，例如n=10时候,(0,9),(1,8),(2,7),(3,6),(4,5)各自形成lists[0],lists[1]...lists[4]
    while (n>=2) {
        for (int i=0;i<n/2;i++) {
            lists[i]=mergeTwoLists(lists[i],lists[n-i-1]);
        }

        //每一次归并后，n会减半
        //如果n是奇数，加1凑成偶数
        if (n%2==0) n=n/2;
        else n=n/2+1;
    }

    return lists[0];
}


/*
//2)递归形式
struct ListNode* merge(struct ListNode** lists,int start,int end) {
    if (start>end) return NULL;
    if (start==end) return lists[start];

    int mid=(start+end)/2;
    struct ListNode* left=merge(lists,start,mid);
    struct ListNode* right=merge(lists,mid+1,end);

    return mergeTwoLists(left,right);
}

struct ListNode* mergeKLists(struct ListNode* lists,int listsSize) {
    return merge(lists,0,listsSize-1);
}
*/



//方法二：排序后相连(堆)
//思路，K个链表（由小到大排序了），每一次分别各个链表取首节点的val，建立最小堆，获得最小值；以此类推
//每个值都要取一次，一共取nk次。每次更新优先队列要logk的复杂度。
//总时间复杂度为O(nklogk)；空间复杂度为优先队列所占空间，为O(k)。

//例如[[1,2,3],[5,6,7],[4],[8,9,10]]
//第一次取1，5，4，8建堆，得最小值为1  余下的[[2,3],[5,6,7],[4],[8,9,10]]
//第二次取2，5，4，8建堆，得最小值为2，余下的[[3],[5,6,7],[4],[8,9,10]]
//以此类推，分别存储


//参考答案：https://leetcode.com/problems/merge-k-sorted-lists/discuss/10562/Share-My-C-code-in-13ms-use-min-heap.
//不是很理解（先放着）
void HeapSort(struct ListNode** lists, int parent,int size) {
    int left=parent*2+1;
    int right=parent*2+2;
    int child;

    //序号从0开始，最大是size-1
    if (left>=size) return;

    if (right>=size) child=left;
    else {
        child=(lists[left]->val < lists[right]->val) ? left:right;
    }

    if (lists[parent]->val > lists[child]->val) {
        struct ListNode* temp=lists[parent];
        lists[parent]=lists[child];
        lists[child]=temp;
        HeapSort(lists,child,size);
    }
}

void BuildHeap(struct ListNode **lists,int size)
{
	for(int i=(size-1)/2;i>=0;--i){
		HeapSort(lists,i,size);
	}
}


struct ListNode *mergeKLists(struct ListNode** lists, int listsize) {
	if(listsize==0) return NULL;

	struct ListNode *head = (struct ListNode*)malloc(sizeof(struct ListNode));
	struct ListNode *int_max = (struct ListNode*)malloc(sizeof(struct ListNode));
	int_max->val = INT_MAX;
	int_max->next = NULL;

    struct ListNode *travel = head;

    //去除lists中的空指针，变为int_max节点
    for(int i=0;i<listsize;++i){
		if(lists[i]==NULL)
			lists[i] = int_max;
	}

	BuildHeap(lists,listsize);

    //即lists[0]不是NULL
    //去掉所有NULL位置
	while(lists[0]!=int_max){
		travel->next = lists[0];

		lists[0] = lists[0]->next;
        travel = travel->next;

        if(lists[0]==NULL)
			lists[0] = int_max;

		HeapSort(lists,0,listsize);
	}

	travel->next = NULL;
	return head->next;
}
```