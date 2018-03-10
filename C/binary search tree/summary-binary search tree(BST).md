## 二叉搜索树
<br>
<br>
<br>

### 0、定义及性质
二叉搜索树（BST binary search tree）：又叫二叉排序树或者二叉查找树，其满足以下性质
- 非空**左子树**所有值**小于**根节点值
- 非空**右子树**所有值**大于**根节点值
- 左、右子树都是二叉搜索树

<br>

由上可以推出：

- BST**最小值**一定在最**左**端端点上，**最大值**一定在最**右**端端点上
- 通过二叉树的**中序遍历**，可以获得由小到大有序排列的序列

<br>
<br>

### 1、查找Find
```c
struct TreeNode* Find(struct TreeNode* root,ElementType x) {
    if (root==NULL) return NULL;
    if (x < root->val)
        root->left=Find(root->left,x);
    else if (x > root->val)
        root->right=Find(root->right,x);
    else
        return root;
}
```
<br>

### 2、查找最大/最小值find Max/find Min
```c
struct TreeNode* findMin(struct TreeNode* root) {
    if (root==NULL) return NULL;
    while (root->left)
        root=root->left;

    return root;
}

struct TreeNode* findMax(struct TreeNode* root) {
    if (root==NULL) return NULL;
    while (root->right)
        root=root->right;

    return root;
}
```
<br>


### 3、插入Insert
```c
//这里使用递归插入，还是比较巧妙
struct TreeNode* Insert(struct TreeNode* root,ElementType x) {
    if (root==NULL) {
        struct TreeNode* root=(struct TreeNode*)malloc(sizeof(struct TreeNode));
        root->val=x;
        root->left=root->right=NULL;
    }
    else {
        if (x < root->val){
            root->left=Insert(root->left,x);
        }
        else if (x > root->val) {
            root->right=Insert(root->right,x);
        }
    }

    return root;
}
```
<br>


### 4、删除delete

![](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/BST001.jpg)
<br>

![](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/BST002.jpg)
<br>

![](https://github.com/LUCY78765580/Day-Day-Leetcode/raw/master/screenshorts/BST003.jpg)
<br>
<br>

```c
struct TreeNode* findMin(struct TreeNode* root) {
    if (root==NULL) return NULL;
    while (root->left)
        root=root->left;

    return root;
}

struct TreeNode* delete(struct TreeNode* root,ElementType x) {
    if (root==NULL) return NULL;
    //左、右子树分别递归删除
    else if (x < root->val) {
        root->left=delete(root->left,x);
    }
    else if (x > root->val) {
        root->right=delete(root->right,x);
    }
    else {
        //找到要删除的点
        //找到改点右子树的最小节点temp，并赋值给当前的root
        //然后递归删除掉temp
        if (root->left&&root->right) {
            struct TreeNode* temp=findMin(root->right);
            root->val=temp->val;
            root->right=delete(root->right,temp->val);
        }
        else {
            //只有右儿子、无子节点
            //只有左儿子、无子节点
            struct TreeNode* temp=root;
            if (root->left==NULL)
                root=root->right;
            else if (root->right==NULL)
                root=root->left;
            free(temp);
        }
    }

    return root;
}
```

