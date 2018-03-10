## 071 Simplify Path
[题目地址](https://leetcode.com/problems/simplify-path/description/)
<br>
<br>
<br>

```c
//简化路径
/*unix style path的规则如下：
/   -> root
/a  -> in (a)

. -> THIS dir path
/a/./   -> still in /a
/a/./b  -> in /a/b

.. -> go "up" one level
/a/./b/..      -> /a/b/..   -> /a
/a/./b/../..   -> /a/..     -> /
/a/./b/../../c              -> /c
*/

/*
strtok():用来将字符串分割成一个个片段。参数s 指向欲分割的字符串，参数delim 则为分割字符串，当strtok()在参数s 的字符串中发现到参数delim 的分割字符时则会将该字符改为\0 字符。在第一次调用时，strtok()必需给予参数s 字符串，往后的调用则将参数s 设置成NULL。每次调用成功则返回下一个分割后的字符串指针
详见博客：http://c.biancheng.net/cpp/html/175.html

strcpy和strcat
1) strcat是用来连接两个字符串的，原型是char *strcat(char *dest,char *src)，作用是把src所指字符串添加到dest结尾处(覆盖dest结尾处的'\0')并添加'\0'。
2) strcpy是用来把字符串拷贝到指定的地方的，原型是char *strcpy(char *dest,const char *src)，作用是把从src地址开始且含有NULL结束符的字符串复制到以dest开始的地址空间。
3) 注意strcat是从dest的结尾处开始操作的，而strcpy是直接覆盖dest指向的内容。

*/


//方法一：使用strtok函数(或者split函数)分割字符串，存入新数组res中
//其实这里的res已经是变相的堆栈

//如果前面是'/'则改为'\0'变成结尾，否则继续--index了
void removeChars(char* res,int index) {
    while (index>=0&&res[--index]!='/') {
        ;
    }

    res[index]='\0';
}

char* simplifyPath(char* path) {
    int len=strlen(path);
    char* res=(char*)calloc(len,sizeof(char));
    int resIndex=0;

    char* tmpStr;

    //注意strtok切割函数，"/"不可写为'/'
    while ((tmpStr=strtok(path,"/"))!=NULL) {
        //如果是"."，为同级目录，不变
        if(strcmp(tmpStr,".")==0 || strlen(tmpStr)==0) {
            ;
        }

        //如果是"..",返回上级目录，需要往上删除
        else if(strcmp(tmpStr,"..")==0){
            removeChars(res,resIndex);
        }

        //如果是其它，原样拷贝到res
        else {
            res[resIndex++] = '/';
            strcpy(res+resIndex,tmpStr);
        }

        //复制后长度改变，resIndex重新计算
        //strtok函数首次参数为path，往后参数皆设置为NULL,即为上一次分割的结尾
        resIndex = strlen(res);
        path=NULL;
    }

    //resIndex为0，原str为空字符串
    if (!resIndex) {
        res[resIndex++]='/';
        res[resIndex]='\0';
    }

    return res;
}



//方法二：构造堆栈
//没有上面简便，不赘述，本质上法1也是堆栈的变相
/*
重复连续出现的’/’，只按1个处理，即跳过重复连续出现的’/’；
1）如果路径名是”.”，则不处理；
2）如果路径名是”..”，则需要弹栈（到上一层），如果栈为空，则不做处理；
3）如果路径名为其他字符串，入栈。

最后，再逐个取出栈中元素（即已保存的路径名），用’/’分隔并连接起来.
*/
```