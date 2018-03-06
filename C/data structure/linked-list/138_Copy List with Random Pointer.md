## 138  Copy List with Random Pointer
[题目地址](https://leetcode.com/problems/copy-list-with-random-pointer/description/)
<br>
<br>
<br>

```c
/**
 * Definition for singly-linked list with a random pointer.
 * struct RandomListNode {
 *     int label;
 *     struct RandomListNode *next;
 *     struct RandomListNode *random;
 * };
 */

//审题：给定一个特殊的单链表，链表的每一个节点多了一个随机指针域，随机指向链表中的某一个节点。要求复制这个链表
//参考博客：http://blog.csdn.net/linhuanmars/article/details/22463599/ ，讲的不错的
/*
前面我们需要一个哈希表的原因是当我们访问一个结点时可能它的随机指针指向的结点还没有访问过，结点还没有创建，所以我们需要线性的额外空间。想避免使用额外空间，我们只能通过利用链表原来的数据结构来存储结点。基本思路是这样的，对链表进行三次扫描，第一次扫描对每个结点进行复制，然后把复制出来的新节点接在原结点的next，也就是让链表变成一个重复链表，就是新旧更替；第二次扫描中我们把旧结点的随机指针赋给新节点的随机指针，因为新结点都跟在旧结点的下一个，所以赋值比较简单，就是node.next.random = node.random.next，其中node.next就是新结点，因为第一次扫描我们就是把新结点接在旧结点后面。现在我们把结点的随机指针都接好了，最后一次扫描我们把链表拆成两个，第一个还原原链表，而第二个就是我们要求的复制链表。因为现在链表是旧新更替，只要把每隔两个结点分别相连，对链表进行分割即可。这个方法总共进行三次线性扫描，所以时间复杂度是O(n)。而这里并不需要额外空间，所以空间复杂度是O(1)
*/

//第1步：复制每个节点并将其附加到原始节点;
//第2步：迭代新列表并修复随机指针
//第3步：分开列表。
//补充：也可以利用哈希map来保存random节点之间的关系，通过递归或非递归来实现。

//真的好难理解呀
struct RandomListNode *copyRandomList(struct RandomListNode *head) {
	if(NULL==head) return head;

	struct RandomListNode *p=head;
	struct RandomListNode *pt;

	//拷贝
    while (p) {
        //这里严格用p->next表示pt，而不是直接用pt
        //主要为了和下面修复过程，p->next->random=p->random->next对齐
        //写成如下也是可以的,但是修复过程中，不可写成pt->random=p->random->next ？？为什么
        /*
        pt=malloc(sizeof(struct RandomListNode));
        pt->label=p->label;
        pt->random=NULL;
        pt->next=p->next;
        p->next=pt;
        p=p->next->next;
        */
        pt=p->next;
		p->next=malloc(sizeof(struct RandomListNode));
		p->next->label=p->label;
		p->next->next=pt;
		p->next->random=NULL;
		p=pt;
    }

	//修复
	p=head;
	while (p) {
		if(p->random){
			p->next->random=p->random->next;
		}
		p=p->next->next;
	}

	//分离
    //初始化一个虚拟表头
    struct RandomListNode* copyed_head==NULL;
    p=head;
    //分离过程，和奇偶链表那一块类似
    //参考：https://leetcode.com/problems/odd-even-linked-list/description/
    while (p) {
        if (copyed_head==NULL) {
            copyed_head=p->next;
            pt=copyed_head;
        }
        else {
            pt->next=p->next;
            pt=pt->next;
        }

        p->next=p->next->next;
        p=p->next;
    }

    return copyed_head;
}
```