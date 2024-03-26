---
title: KMP算法
toc: true
date: 
cover: http://say9y2uwt.hn-bkt.clouddn.com/myblogPicture/%E6%98%8E%E6%97%A5%E6%96%B9%E8%88%9F%E5%B0%8F%E4%BA%BA5.jpg
categories: 算法
description: 想不出来介绍了
tags: KMP
post_meta:
  page:
    date_type: both # created or updated or both 主頁文章日期是創建日或者更新日或都顯示
    date_format: relative # date/relative 顯示日期還是相對日期
    categories: true # true or false 主頁是否顯示分類
    tags: true # true or false 主頁是否顯示標籤
    label: false # true or false 顯示描述性文字
  post:
    date_type: both # created or updated or both 文章頁日期是創建日或者更新日或都顯示
    date_format: relative # date/relative 顯示日期還是相對日期
    categories: true # true or false 文章頁是否顯示分類
    tags: true # true or false 文章頁是否顯示標籤
    label: false # true or false 顯示描述性文字
copyright_author: Elysian剑彧
copyright_author_href: https://dcelysia.netlify.app/
copyright_url: https://dcelysia.netlify.app/
copyright_info: 此文章版权归Elysian剑彧所有，如有转载，请注明来自原作者
---

# KMP算法

KMP是在一个字符串里寻找是否有另一个字符串并返回子串的下标，如果没有则返回-1，但时间复杂度为O(n)。

## 暴力思路

暴力思路非常好想，就是从当前下标往后遍历，如果与寻找的字符串相同就继续遍历，如果不相同则放弃，然后从当前下标的下一个继续来遍历，这样的时间复杂度在特殊情况下会很高，例如

```
字符串1 1111111111111111111111111111111111111112
字符串2 111111112
```

假设这里字符串1长度为m,字符串2长度为n，则时间复杂度大概是O(n*m)。



## KMP算法加速原理

KMP代码（为了之后复习不往后面翻，把代码贴前面)~~看起来就特简单~~
```cpp
int Next[200];
void getNext(string str2){
    int len = str2.size();
    Next[0] = -1;
    if(len == 1) return;
    Next[1] = 0;
    int cn = 0;
    int i = 2;
    while(i < len){
        if(str2[cn] == str2[i-1]) Next[i++] = ++cn;
        else if(cn > 0) cn = Next[cn];
        else Next[i++] = 0;
    }
    return;
}
int kmp(string& str1, string& str2){
    int i1 = 0;
    int i2 = 0;
    getNext(str2);
    while(i1 < str1.size() && i2 < str2.size()){
        if(str1[i1] == str2[i2]) i1++,i2++;
        else if(i2 > 0) i2 = Next[i2];
        else i1++;
    }
    return i2 == str2.size() ? i1 - i2 : -1;
}
```

### 求next数组

在运用KMP之前，我们要求  **字符串2的** **当前下标的**  **前面字符子串的** **最长前缀和后缀相等的长度** 并把他们存到一个next数组里面~~是有点小乱~~

举个例子就明白了

```
abbcabbe 假设求这个字符串的next数组
a b b c a b b e 只是为了好看加了空格，原本字符串的内容没有改变
0 1 2 3 4 5 6 7 下标
```

首先next[0]为-1，因为0之前没有前面的字符串了，所以人为规定为-1

next[1]为0， 因为1前面字符串长度为1， 也就是0下标的那个字符 ，因为本身在这里不算是前缀 和 后缀，所以规定为0 ， 这里还不是很明白为什么也可以先往后看

next[2]为0， 下标2前面的字符子串就是从下标(0~1)的子串，前缀为a ，后缀为b,不相等所以这里为0

next[3]为0， 子串前缀可以分为(a,ab),后缀可以分为(b,bb)都不相等，所以也为0

同理，next[4]也为0，next[5]为1(前缀a和后缀a相等，最长的相同前后缀为1)

next[6]为2(前缀为ab,后缀为ab)

next[7]为3(前缀abb,后缀abb)

### 开始加速

两个字符串进行比较，如果两个字符串下标的字符都相等，则两个下标都++往前走

这里关键是如果不相等了，字符串2的下标就会变成当前下标的next数组存的值

直接举例子简单一点

```
ababababc
    |
ababc
    |这里不相等的(此时下标的next数组的值是2，所以字符串2下标从4变成2)
next数组
-1 0 0 1 2
----------------------------------------------
abababc
    |
ababc
  |
此时继续往下比
```

跳过了其实就是前缀的长度，也就是next数组存的长度，就是前面的ab不用比了，两者一定是相同

为什么？ 因为我只有相等了两个下标会往后移，所以在下标4的地方两个字符串才不相等，意味着下标4前面的下标2 和 下标3 两个字符一定也相等(这里只说前面2个只是为了解释后面next加速)，由于下标4的next数组为2，就是最长前缀和后缀长度是2，所以字符串1在 下标4的下标2,3 一定会等于 字符串2的前缀0,1。所以字符串2直接从下标3的地方开始比

如果这时候往前移了还是不相等 ，还是上面的例子我改一下

```
ababebc
    |
ababc
  |(此时继续把下标2改成next数组里的数据1)
此时继续往下比
-------------------------------------
ababebc
    |
ababc
 |(从1下标开始比,还是不相等继续下标为next数组为0)
```

然后如果字符串2下标都判断到0了都不行，字符串1你就别守着这个下标不放了，直接i1++(i1就是字符串1的下标)

代码特别简单

```cpp
int kmp(string& str1, string& str2){
    
    int i1 = 0;
    int i2 = 0;
    getNext(str2);//这个先不管，就是求next数组的黑盒
    while(i1 < str1.size() && i2 < str2.size()){
        if(str1[i1] == str2[i2]) i1++,i2++;//相等都++
        else if(i2 > 0) i2 = Next[i2];//不相等i2为next数组的值
        else i1++;//i2都为0了，i1直接++
    }
    return i2 == str2.size() ? i1 - i2 : -1;
}
```

### 求next数组

快速求next数组跟动态规划差不多，你想如果前面的next数组都求出来了，下标cur的next数组就可以看

str[next[cur-1]] 等不等于 str[cur-1] 如果相等了next[cur] = next[cur] + 1;

如果不相等了，str[cur-1] 就要和 str[next[next[cur]]] 比较

举个例子

````
abcabce
      |(我现在求这个下标的next)
  |  |(这里是cur-1,此下标的next数组值为2)
  |(这里就是str[next[cur-1]])
因为str[next[cur-1]] == str[cur-1] 所以e对着的next数组就是 2 + 1
````

如果不相等了，就往前跳，类似于kmp，如果比较的那个下标已经为0了，就不能往前跳了(但是可以跳到0位置，就跟下面这个例子一样)

````
abcabae
      |(我现在求这个下标的next)
  |  |(这里是cur-1,此下标的next数组值为2)
  |(这里就是str[next[cur-1]])
因为a和c不相等，往前跳，但c对着的next为0，所以跳到0
此时str[0] == str[cur-1] ,所以e下标的next为1
````

看代码简单点

```cpp
int Next[200];
void getNext(string str2){
    int len = str2.size();
    Next[0] = -1;
    if(len == 1) return;
    Next[1] = 0;
    int cn = 0;
    int i = 2;
    while(i < len){
        if(str2[cn] == str2[i-1]) Next[i++] = ++cn;
        else if(cn > 0) cn = Next[cn];
        else Next[i++] = 0;
    }
    return;
}
```

这样就讲完了

感谢观看