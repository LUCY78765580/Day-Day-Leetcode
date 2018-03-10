## 096  Unique Binary Search Trees
[题目地址](https://leetcode.com/problems/unique-binary-search-trees/description/)
<br>
<br>
<br>

```c
//可行的二叉查找树(数量)

//memset()函数，用来将指定内存的前n个字节设置为特定的值
/*
其实二叉查找树可以任意取根，只要满足中序遍历有序的要求就可以。从处理子问题的角度来看，选取一个结点为根，就把结点切成左右子树，以这个结点为根的可行二叉树数量就是左右子树可行二叉树数量的乘积，所以总的数量是将以所有结点为根的可行结果累加起来。写成表达式如下：
这正是卡特兰数的一种定义方式，是一个典型的动态规划的定义方式（根据其实条件和递推式求解结果）。所以维护量res[i]表示含有i个结点的二叉查找树的数量。根据上述递推式依次求出1到n的的结果即可。
时间上每次求解i个结点的二叉查找树数量的需要一个i步的循环，总体要求n次，所以总时间复杂度是O(1+2+...+n)=O(n^2)。空间上需要一个数组来维护，并且需要前i个的所有信息，所以是O(n)。代码如下：
(卡特兰数：https://baike.baidu.com/item/%E5%8D%A1%E7%89%B9%E5%85%B0%E6%95%B0/6125746?fr=aladdin)
*/

//总的G(n)为所有F(i,n)的和,G(n) = F(1, n) + F(2, n) + ... + F(n, n).
//G(n)+=G[i-1]*G[n-i]
//将n换成i,i换成j,得到G[i]+=G[j-1]*G[i-j]
int numTrees(int n) {
    //注意这里一定要初始化
    int G[n+1];
    memset(G,0,sizeof(int)*(n+1));

    //或者 指针定义+calloc 方式进行初始化
    //int* G=(int*)calloc(n+1,sizeof(int));

    //很巧妙，设置G大小为n+1并将G[0]=1
    G[0]=G[1]=1;

    for (int i=2;i<=n;++i) {
        for (int j=1;j<=i;++j) {
            G[i]+=G[j-1]*G[i-j];
        }
    }

    return G[n];
}


//参考博客2：http://c.biancheng.net/cpp/html/157.html
//参考博客2：http://www.cnblogs.com/xiaolongchase/archive/2011/10/22/2221326.html
```