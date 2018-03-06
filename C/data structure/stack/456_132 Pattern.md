## 455  132 Pattern
[题目地址](https://leetcode.com/problems/132-pattern/description/)
<br>
<br>
<br>

```c
//132样式
//方法一：O(n*3)的复杂度，超时了
/*
bool find132pattern(int* nums, int numsSize) {
    int i,j,k;
    for (i=0;i<numsSize;i++) {
        for (j=i+1;j<numsSize;j++) {
            for (k=j+1;k<numsSize;k++) {
                if (nums[i]<nums[k]&&nums[k]<nums[j]) {
                    return true;
                    break;
                }
            }
        }
    }

    return false;
}
*/


//方法二：使用堆栈
//使用堆栈从数组最右边开始，将数据压入栈
//往左边找，如果找到比peek(stack)大的，即获得最小值Min（此时找到了Min和大于Min的数）
//继续往左，如果找到比Min值要小的，则说明存在132模式

//建栈及相关函数

struct StackNode {
    int val;
    struct StackNode* next;
};

struct StackNode* createStack() {
    struct StackNode* s=malloc(sizeof(struct StackNode));
    s->next=NULL;

    return s;
}

bool isEmpty(struct StackNode* s) {
    return s->next==NULL;
}

void push(struct StackNode* s,int x) {
    struct StackNode* temp=malloc(sizeof(struct StackNode));
    temp->val=x;
    temp->next=s->next;
    s->next=temp;
}

int pop(struct StackNode* s) {
    struct StackNode* temp;
    int tmp;

    if (!isEmpty(s)) {
        temp=s->next;
        tmp=temp->val;
        s->next=temp->next;

        free(temp);
        return tmp;
    }
    return NULL;
}

int peek(struct StackNode* s) {
    if (!isEmpty(s)) {
        return s->next->val;
    }
    return NULL;
}


//主函数
bool find132pattern(int* nums,int numsSize) {
    struct StackNode* stack=createStack();

    int i=numsSize-1;
    int Min=INT_MIN;

    while (i>=0) {
        if (nums[i]<Min) return true;
        else while (!isEmpty(stack)&&nums[i]>peek(stack)) {
            Min=pop(stack);
        }

        push(stack,nums[i]);
        i--;
    }

    return false;
}
```