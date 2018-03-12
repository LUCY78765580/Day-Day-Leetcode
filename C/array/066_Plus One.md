## 066 Plus One
[题目地址](https://leetcode.com/problems/plus-one/description/)
<br>
<br>
<br>

```c
/**
 * Return an array of size *returnSize.
 * Note: The returned array must be malloced, assume caller calls free().
 */


//数组加1

//最高有效数字(就是1)位于列表头部
//遍历时分两种情况就行了（小于9的情况和等于9的情况）

int* plusOne(int* digits,int digitsSize,int* returnSize) {
    int i=digitsSize-1;

    for (i;i>=0;i--) {
        if (digits[i]<9) {
            digits[i]++;
            *returnSize=digitsSize;
            return digits;
            //注意这里要return digits
            //以及附带的*returnSize=digitsSize
        }
        else {
            digits[i]=0;
        }
    }

    //"the most significant digit is at the head of the list"
    int* res=(int*)malloc((digitsSize+1)*sizeof(int));
    res[0]=1;
    for (int j=1;j<digitsSize+1;j++) {
        res[j]=*digits;
        digits++;
    }

    *returnSize=digitsSize+1;
    return res;
}



//方法二：使用递归
//参考discuss:https://leetcode.com/problems/plus-one/discuss/24320/JAVA-recursion-solution
/* java 递归版本
public class Solution {
    public int[] plusOne(int[] digits) {
        return helper(digits,digits.length-1);
    }

    private int[] helper(int[] digits, int index){
        if(digits[index] < 9){
            digits[index]++;
            return digits;
        }else{
            if(index != 0){
                digits[index] = 0;
                return helper(digits,index-1);
            }else{
                int[] res = new int[digits.length+1];
                res[0] = 1;
                return res;
            }
        }
    }
}
*/
```
