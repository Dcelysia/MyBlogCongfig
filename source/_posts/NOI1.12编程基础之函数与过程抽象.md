---
title: NOI1.12编程基础之函数与过程抽象
toc: true
date: 
cover: /img/明日方舟小人5.jpg
categories: 题解
description: NOI的题解
tags: 后端
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



# NOI**1.12编程基础之函数与过程抽象**

其实这10道题全部都是模拟或者模拟加优化，为了稍微练一下cpp所以有些简单题目我是用cpp做的，这几道题也没什么算法可讲，用java也很好实现

## 01:简单算术表达式求值

- 总时间限制: 

  1000ms

- 内存限制: 

  65536kB

- 描述

  两位正整数的简单算术运算（只考虑整数运算），算术运算为：+，加法运算； -，减法运算； *，乘法运算； /，整除运算； %，取余运算。算术表达式的格式为（运算符前后可能有空格）： 运算数 运算符 运算数请输出相应的结果。 

- 输入

  一行算术表达式。

- 输出

  整型算数运算的结果（结果值不一定为2位数，可能多于2位或少于2位）。

- 样例输入

  `32+64`

- 样例输出

  `96`

不管用java还是cpp都很好做

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main{
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String str = br.readLine();
        int index = 0;
        str = str.replaceAll("[ ]+", "");
        int num1 = 0;
        int num2 = 0;
        while (true) {
            if (str.charAt(index) == '+') {
                num1 = Integer.parseInt(str.substring(0, index));
                num2 = Integer.parseInt(str.substring(index + 1, str.length()));
                System.out.println(num1+num2);
                break;
            }else if(str.charAt(index) == '-'){
                num1 = Integer.parseInt(str.substring(0, index));
                num2 = Integer.parseInt(str.substring(index + 1, str.length()));
                System.out.println(num1-num2);
                break;
            }else if(str.charAt(index) == '*'){
                num1 = Integer.parseInt(str.substring(0, index));
                num2 = Integer.parseInt(str.substring(index + 1, str.length()));
                System.out.println(num1*num2);
                break;
            }else if(str.charAt(index) == '/'){
                num1 = Integer.parseInt(str.substring(0, index));
                num2 = Integer.parseInt(str.substring(index + 1, str.length()));
                System.out.println(num1/num2);
                break;
            }else if(str.charAt(index) == '%'){
                num1 = Integer.parseInt(str.substring(0, index));
                num2 = Integer.parseInt(str.substring(index + 1, str.length()));
                System.out.println(num1%num2);
                break;
            }
            index ++;
        }
    }
}
```



## 02:短信计费

描述

用手机发短信，一条短信资费为0.1元，但限定一条短信的内容在70个字以内（包括70个字）。如果你一次所发送的短信超过了70个字，则会按照每70个字一条短信的限制把它分割成多条短信发送。假设已经知道你当月所发送的短信的字数，试统计一下你当月短信的总资费。

输入

第一行是整数n，表示当月发送短信的总次数，接着n行每行一个整数，表示每次短信的字数。

输出

输出一行，当月短信总资费，单位为元，精确到小数点后1位。

样例输入

```java
10
39
49
42
61
44
147
42
72
35
46
```

样例输出

```java
1.3
```

AC代码

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main{
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine());
        double sum = 0;
        for(int i = 0;i<n;i++){
            int tmp = Integer.parseInt(br.readLine());
            if(tmp%70 == 0){
                sum += 0.1*(tmp/70);
            }else{
                sum += 0.1*(tmp/70+1);
            }
        }
        System.out.printf("%.1f",sum);
    }
}
```

## 03:甲流病人初筛

描述

目前正是甲流盛行时期，为了更好地进行分流治疗，医院在挂号时要求对病人的体温和咳嗽情况进行检查，对于体温超过37.5度（含等于37.5度）并且咳嗽的病人初步判定为甲流病人（初筛）。现需要统计某天前来挂号就诊的病人中有多少人被初筛为甲流病人。

输入

第一行是某天前来挂号就诊的病人数n。（n < 200）
其后有n行，每行是病人的信息，包括三个信息：姓名（字符串，不含空格，最多8个字符）、体温（float）、是否咳嗽（整数，1表示咳嗽，0表示不咳嗽）。每行三个信息之间以一个空格分开。

输出

按输入顺序依次输出所有被筛选为甲流的病人的姓名，每个名字占一行。之后在输出一行，表示被筛选为甲流的病人数量。

样例输入

```
5
Zhang 38.3 0
Li 37.5 1
Wang 37.1 1
Zhao 39.0 1
Liu 38.2 1
```

样例输出

```
Li
Zhao
Liu
3
```

AC代码

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;

public class Main{
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        ArrayList<String> list = new ArrayList<>();
        int n = Integer.parseInt(br.readLine());
        int sum = 0;
        for(int i = 0;i<n;i++){
            String[] form = br.readLine().split(" ");
            Double f = Double.parseDouble(form[1]);
            if(f>=37.5&&form[2].equals("1")){
                list.add(form[0]);
                sum++;
            }
        }
        for (String person : list) {
            System.out.println(person);  
        }
        System.out.println(sum);
    }
}
```

## 04:最匹配的矩阵

描述

给定一个m*n的矩阵A和r*s的矩阵B,其中0 < r ≤ m, 0 < s ≤ n，A、B所有元素值都是小于100的正整数。求A中一个大小为r*s的子矩阵C，使得B和C的对应元素差值的绝对值之和最小，这时称C为最匹配的矩阵。如果有多个子矩阵同时满足条件，选择子矩阵左上角元素行号小者，行号相同时，选择列号小者。

输入

第一行是m和n，以一个空格分开。
之后m行每行有n个整数，表示A矩阵中的各行，数与数之间以一个空格分开。
第m+2行为r和s，以一个空格分开。
之后r行每行有s个整数，表示B矩阵中的各行，数与数之间以一个空格分开。
（1 ≤ m ≤ 100,1 ≤ n ≤ 100）

输出

输出矩阵C，一共r行，每行s个整数，整数之间以一个空格分开。

样例输入

```
3 3
3 4 5
5 3 4
8 2 4
2 2
7 3
4 9
```

样例输出

```
4 5 
3 4 
```



这道题看到n和m的范围其实就可以考虑穷举了，这道题我怕java太慢一开始就是用cpp做的，就是从右下角往左上角走，如果小于等于最小值就记录下右下角那个点，打印的时候也用右下角那个点往上和左推，因为从右下到左上，所以枚举出来的矩形一定是最左边的

```c++
#include <iostream>
#include <string>
#include <algorithm>
#include <bits/stdc++.h>
using namespace std;
int a[110][110];
int b[110][110];
int main()
{
    int n, m, r, s;
    cin >> n >> m;
    long long min = 99999999;
    int oi = -1;
    int oj = -1;
    for (int i = 0; i < n; i++)
    {
        for (int j = 0; j < m; j++)
        {
            cin >> a[i][j];
        }
    }
     cin >> r >> s;
    for (int i = 0; i < r; i++)
    {
        for (int j = 0; j < s; j++)
        {
            cin >> b[i][j];
        }
    }
    for (int i = n - 1; i - r + 1 >= 0; i--)
    {
        for (int j = m - 1; j - s + 1 >= 0; j--)
        {
            long long tmp = 0;
            int tmpr = r - 1;
            for (int l = i; l >= i - r + 1; l--)
            {
                int tmps = s - 1;
                for (int h = j; h >= j - s + 1; h--)
                {
                    int d = a[l][h] - b[tmpr][tmps];
                    if(d<0){
                        d = -d;
                    }
                    tmp += d;
                    tmps--;
                }
                tmpr--;
            }
            if (tmp <= min)
            {
                min = tmp;
                oi = i;
                oj = j;
            }
        }
    }

    for (int i = oi - r + 1; i <= oi; i++)
    {
        for (int j = oj - s + 1; j <= oj; j++)
        {
            cout << a[i][j] << " ";
        }
        cout << endl;
    }

    return 0;
}
```

## 05:统计单词数

描述

一般的文本编辑器都有查找单词的功能，该功能可以快速定位特定单词在文章中的位置，有的还能统计出特定单词在文章中出现的次数。现在，请你编程实现这一功能，具体要求是：给定一个单词，请你输出它在给定的文章中出现的次数和第一次出现的位置。**注意：匹配单词时，不区分大小写，但要求完全匹配，即给定单词必须与文章中的某一独立单词在不区分大小写的情况下完全相同（参见样例1），如果给定单词仅是文章中某一单词的一部分则不算匹配（参见样例2）。**

输入

2 行。 第 1 行为一个字符串，其中只含字母，表示给定单词； 第 2 行为一个字符串，其中只可能包含字母和空格，表示给定的文章。

输出

只有一行，如果在文章中找到给定单词则输出两个整数，两个整数之间用一个空格隔开，分别是单词在文章中出现的次数和第一次出现的位置（即在文章中第一次出现时，单词首字母在文章中的位置，位置从0开始）；如果单词在文章中没有出现，则直接输出一个整数-1。

样例输入

`样例 #1： To to be or not to be is a question 样例 #2： to Did the Ottoman Empire lose its power at that time`

样例输出

`样例 #1： 2 0 样例 #2： -1`

Ac代码

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.HashMap;

public class Main{
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String find = br.readLine();
        String[] strs = br.readLine().split(" ");
        int count = 0;
        boolean flag = false;
        int length = 0;
        for (int i = 0; i < strs.length; i++) {
            if (strs[i].equalsIgnoreCase(find)) {
                count++;
            } else {
                if (count != 0) {
                    flag = true;
                }
                if (!flag) {
                    length += strs[i].length() + 1;
                }
            }
        }
        if(count == 0){
            System.out.println("-1");
        }else{
            System.out.println(count+" "+length);
        }
    }
}
```

## 06:寻宝

描述

传说很遥远的藏宝楼顶层藏着诱人的宝藏。小明历尽千辛万苦终于找到传说中的这个藏 宝楼，藏宝楼的门口竖着一个木板，上面写有几个大字：寻宝说明书。说明书的内容如下：

藏宝楼共有 N+1 层，最上面一层是顶层，顶层有一个房间里面藏着宝藏。除了顶层外，藏宝楼另有 N 层，每层 M 个房间，这 M 个房间围成一圈并按逆时针方向依次编号为 0，…， M-1。其中一些房间有通往上一层的楼梯，每层楼的楼梯设计可能不同。每个房间里有一个指示牌，指示牌上有一个数字 x，表示从这个房间开始按逆时针方向选择第 x 个有楼梯的房间（假定该房间的编号为 k），从该房间上楼，上楼后到达上一层的 k 号房间。比如当前房间的指示牌上写着 2，则按逆时针方向开始尝试，找到第 2 个有楼梯的房间，从该房间上楼。如果当前房间本身就有楼梯通向上层，该房间作为第一个有楼梯的房间。

寻宝说明书的最后用红色大号字体写着：“寻宝须知：帮助你找到每层上楼房间的指示牌上的数字（即每层第一个进入的房间内指示牌上的数字）总和为打开宝箱的密钥”。

请帮助小明算出这个打开宝箱的密钥。

输入

第一行 2 个整数 N 和 M，之间用一个空格隔开。N 表示除了顶层外藏宝楼共 N 层楼， M 表示除顶层外每层楼有 M 个房间。
接下来 N*M 行，每行两个整数，之间用一个空格隔开，每行描述一个房间内的情况，其中第(i-1)*M+j 行表示第 i 层 j-1 号房间的情况（i=1, 2, …, N；j=1, 2, … ,M）。第一个整数表示该房间是否有楼梯通往上一层（0 表示没有，1 表示有），第二个整数表示指示牌上的数字。注意，从 j 号房间的楼梯爬到上一层到达的房间一定也是 j 号房间。
最后一行，一个整数，表示小明从藏宝楼底层的几号房间进入开始寻宝（注：房间编号从 0 开始）。

对于50%数据，有 0< N ≤ 1000，0 < x ≤ 10000；
对于100%数据，有 0 < N ≤ 10000，0 < M ≤ 100，0 < x ≤ 1,000,000。

输出

输出只有一行，一个整数，表示打开宝箱的密钥，这个数可能会很大，请输出对 20123 取模的结果即可。

样例输入

```
2 3
1 2
0 3
1 4
0 1
1 5
1 2
1
```

样例输出

```
5
```

提示

输入输出样例说明：

第一层：
0 号房间，有楼梯通往上层，指示牌上的数字是 2；
1 号房间，无楼梯通往上层，指示牌上的数字是 3；
2 号房间，有楼梯通往上层，指示牌上的数字是 4；

第二层：
0 号房间，无楼梯通往上层，指示牌上的数字是 1；
1 号房间，有楼梯通往上层，指示牌上的数字是 5；
2 号房间，有楼梯通往上层，指示牌上的数字是 2；

小明首先进入第一层（底层）的 1 号房间，记下指示牌上的数字为 3，然后从这个房间 开始，沿逆时针方向选择第 3 个有楼梯的房间 2 号房间进入，上楼后到达第二层的 2 号房间， 记下指示牌上的数字为 2，由于当前房间本身有楼梯通向上层，该房间作为第一个有楼梯的房间。因此，此时沿逆时针方向选择第 2 个有楼梯的房间即为 1 号房间，进入后上楼梯到达 顶层。这时把上述记下的指示牌上的数字加起来，即 3+2=5，所以打开宝箱的密钥就是 5。



这道题不仅要模拟，还要优化，要留心记住每一层有楼梯的多少个，~~这道题把=写成==查了好久~~

```c++
#include <bits/stdc++.h>
using namespace std;
int pd[10010][300], a[10010][300];
int main(void)
{
    int N, M;
    scanf("%d %d", &N, &M);
    int l = 0;
    for (int i = 1; i <= N; i++)
    {
        l = 0;
        for (int j = 0; j < M; j++)
        {
            scanf("%d %d", &pd[i][j], &a[i][j]);
            if (pd[i][j] == 1)
                l++;
        }
        pd[i][M] = l;
    }
    int in;
    scanf("%d", &in);
    long long sum = 0;
    int j = 0;
    for (int i = 1; i <= N; i++)
    {
        sum +=  a[i][in];
        sum %= 20123;
        int k = 0;
        for (j = in; ; j++)
        {
            if(j==M) j = 0;
            
            if(pd[i][j] == 1) k++;
            //这里最关键，是为了防止一个楼梯或者模出来为0的情况
            if(k==(a[i][in]-1)%pd[i][M]+1) break;
        }
        in = j;
    }
    printf("%lld", sum);
    return 0;
}
```

## 07:机器翻译

描述

小晨的电脑上安装了一个机器翻译软件，他经常用这个软件来翻译英语文章。

这个翻译软件的原理很简单，它只是从头到尾，依次将每个英文单词用对应的中文含义来替换。对于每个英文单词，软件会先在内存中查找这个单词的中文含义，如果内存中有，软件就会用它进行翻译；如果内存中没有，软件就会在外存中的词典内查找，查出单词的中文含义然后翻译，并将这个单词和译义放入内存，以备后续的查找和翻译。

假设内存中有M个单元，每单元能存放一个单词和译义。每当软件将一个新单词存入内存前，如果当前内存中已存入的单词数不超过M−1，软件会将新单词存入一个未使用的内存单元；若内存中已存入M 个单词，软件会清空最早进入内存的那个单词，腾出单元来，存放新单词。

假设一篇英语文章的长度为N个单词。给定这篇待译文章，翻译软件需要去外存查找多少次词典？假设在翻译开始前，内存中没有任何单词。

输入

输入文件共2行。每行中两个数之间用一个空格隔开。
第一行为两个正整数M和N，代表内存容量和文章的长度。
第二行为N个非负整数，按照文章的顺序，每个数（大小不超过1000）代表一个英文单词。文章中两个单词是同一个单词，当且仅当它们对应的非负整数相同。

对于10%的数据有M = 1，N ≤ 5。
对于100%的数据有0 < M ≤ 100，0 < N ≤ 1000。

输出

共1行，包含一个整数，为软件需要查词典的次数。

样例输入

```
样例 #1：
3 7
1 2 1 5 4 4 1

样例 #2：
2 10
8 824 11 78 11 78 11 78 8 264
```

样例输出

```
样例 #1：
5

样例 #2：
6
```

提示

输入输出样例 1 说明：

整个查字典过程如下：每行表示一个单词的翻译，冒号前为本次翻译后的内存状况：

空：内存初始状态为空。
1． 1：查找单词1并调入内存。
2． 1 2：查找单词2并调入内存。
3． 1 2：在内存中找到单词1。
4． 1 2 5：查找单词5并调入内存。
5． 2 5 4：查找单词4并调入内存替代单词1。
6． 2 5 4：在内存中找到单词4。
7． 5 4 1：查找单词1并调入内存替代单词2。

这道题好久之前做的了，只要模拟出题目要求就行

```c
#include<stdio.h>

int main(void) {
	int M = 0;
	int N = 0;
	scanf("%d %d", &M, &N);
	int a[1001] = { 0 };
	int s[101] = { 0 };
	int output = 0;
	for (int i = 0; i < N; i++){
		scanf("%d", &a[i]);
	}
	int count = 0;
	int j = 0;
	for (int i = 0; i < N; i++) {
		//判断字典里有没有
		int isCunzai = 0;
		for (j = 0; j < count; j++) {
			if (s[j] == a[i]) {
				isCunzai = 1;
			}
		}
		//如果没有
		if (isCunzai == 0) {
			//查询加1
			output++;
			//如果s存满了
			if (count == M) {
				for (int k = 0; k < M - 1; k++) {
					s[k] = s[k + 1];
				}
				//把最新的存在最后面
				s[M- 1] = a[i];
			}
			else {
				s[count++] = a[i];
			}
		}
	}
	printf("%d", output);	
	return 0;
}
```

## 08:Vigenère密码

[原图有图和详细的说明](http://noi.openjudge.cn/ch0112/08/)

就是简单的往前推就行了

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        char[] key = sc.nextLine().toCharArray();
        char[] secret = sc.nextLine().toCharArray();
        int index = 0;
        for (int i = 0; i < secret.length; i++) {
            if (index == key.length)
                index = 0;
            int t = 0;
            if ('a' <= key[index] && key[index] <= 'z') {
                t = key[index++] - 'a';
            } else {
                t = key[index++] - 'A';
            }
            if ('a' <= secret[i] && secret[i] <= 'z') {
                if (secret[i] - t < 'a') {
                    System.out.print((char) ('z' - t + secret[i] - 'a' +1));
                }else{
                    System.out.print((char)(secret[i] - t));
                }
            } else {
                if (secret[i] - t < 'A') {
                    System.out.print((char) ('Z' - t + secret[i] - 'A' +1));
                }else{
                    System.out.print((char)(secret[i] - t));
                }
            }
        }
    }
}
```

## 09:图像旋转翻转变换

描述

给定m行n列的图像各像素点灰度值，对其依次进行一系列操作后，求最终图像。

其中，可能的操作及对应字符有如下四种：

A：顺时针旋转90度；

B：逆时针旋转90度；

C：左右翻转；

D：上下翻转。

输入

第一行包含两个正整数m和n，表示图像的行数和列数，中间用单个空格隔开。1 <= m <= 100, 1 <= n <= 100。
接下来m行，每行n个整数，表示图像中每个像素点的灰度值，相邻两个数之间用单个空格隔开。灰度值范围在0到255之间。
接下来一行，包含由A、B、C、D组成的字符串s，表示需要按顺序执行的操作序列。s的长度在1到100之间。

输出

m'行，每行包含n'个整数，为最终图像各像素点的灰度值。其中m'为最终图像的行数，n'为最终图像的列数。相邻两个整数之间用单个空格隔开。

样例输入

```
2 3
10 0 10
100 100 10
AC
```

样例输出

```
10 100
0 100
10 10
```



用两个表格能更好的实现，我写的长是长了点，但是其实很多都是复制的

```c++
#include <bits/stdc++.h>
using namespace std;

int main(void)
{
    int a[110][110] = {0};
    int b[110][110] = {0};
    char str[110] = {0};
    int n, m;
    int isA = 1;
    int isB = 0;
    scanf("%d %d", &n, &m);
    int lenIa = n;
    int lenJa = m;
    int lenIb = m;
    int lenJb = n;
    for (int i = 0; i < lenIa; i++)
    {
        for (int j = 0; j < lenJa; j++)
        {
            scanf("%d", &a[i][j]);
        }
    }
    scanf(" %s", str);
    int indexS = 0;
    while (str[indexS] != '\0')
    {
        if (str[indexS] == 'A')
        {
            int indexI = 0;
            int IndexJ = 0;
            if (isA)
            {
                for (int j = 0; j < lenJa; j++)
                {
                    for (int i = lenIa - 1; i >= 0; i--)
                    {
                        b[indexI][IndexJ++] = a[i][j];
                        if (IndexJ == lenJb)
                        {
                            indexI++;
                            IndexJ = 0;
                        }
                    }
                }
                isA = 0;
                isB = 1;
            }
            else
            {
                for (int j = 0; j < lenJb; j++)
                {
                    for (int i = lenIb - 1; i >= 0; i--)
                    {
                        a[indexI][IndexJ++] = b[i][j];
                        if (IndexJ == lenJa)
                        {
                            indexI++;
                            IndexJ = 0;
                        }
                    }
                }
                isA = 1;
                isB = 0;
            }
        }
        else if (str[indexS] == 'B')
        {
            int indexI = 0;
            int IndexJ = 0;
            if (isA)
            {
                for (int j = lenJa - 1; j >= 0; j--)
                {
                    for (int i = 0; i <=lenIa - 1; i++)
                    {
                        b[indexI][IndexJ++] = a[i][j];
                        if (IndexJ == lenJb)
                        {
                            indexI++;
                            IndexJ = 0;
                        }
                    }
                }
                isA = 0;
                isB = 1;
            }
            else
            {
                for (int j = lenJb - 1; j >= 0; j--)
                {
                    for (int i = 0; i <= lenIb - 1; i++)
                    {
                        a[indexI][IndexJ++] = b[i][j];
                        if (IndexJ == lenJa)
                        {
                            indexI++;
                            IndexJ = 0;
                        }
                    }
                }
                isA = 1;
                isB = 0;
            }
        }
        else if (str[indexS] == 'C')
        {
            if (isA)
            {
                for (int i = 0; i < lenIa; i++)
                {
                    for (int j = 0; j < lenJa / 2; j++)
                    {
                        int tmp = a[i][j];
                        a[i][j] = a[i][lenJa - 1 - j];
                        a[i][lenJa - 1 - j] = tmp;
                    }
                }
            }
            else
            {
                for (int i = 0; i < lenIb; i++)
                {
                    for (int j = 0; j < lenJb / 2; j++)
                    {
                        int tmp = b[i][j];
                        b[i][j] = b[i][lenJb - 1 - j];
                        b[i][lenJb - 1 - j] = tmp;
                    }
                }
            }
        }
        else
        {
            if (isA)
            {
                for (int j = 0; j < lenJa; j++)
                {
                    for (int i = 0; i < lenIa / 2; i++)
                    {
                        int tmp = a[i][j];
                        a[i][j] = a[lenIa - 1 - i][j];
                        a[lenIa - 1 - i][j] = tmp;
                    }
                }
            }
            else
            {
                for (int j = 0; j < lenJb; j++)
                {
                    for (int i = 0; i < lenIb / 2; i++)
                    {
                        int tmp = b[i][j];
                        b[i][j] = b[lenIb - 1 - i][j];
                        b[lenIb - 1 - i][j] = tmp;
                    }
                }
            }
        }
        indexS++;
    }
    if (isA)
    {
        for (int i = 0; i < lenIa; i++)
        {
            for (int j = 0; j < lenJa; j++)
            {
                printf("%d ", a[i][j]);
            }
            printf("\n");
        }
    }
    else
    {
        for (int i = 0; i < lenIb; i++)
        {
            for (int j = 0; j < lenJb; j++)
            {
                printf("%d ", b[i][j]);
            }
            printf("\n");
        }
    }
}
```

## 10:素数对

描述

两个相差为2的素数称为素数对，如5和7，17和19等，本题目要求找出所有两个数均不大于n的素数对。

输入

一个正整数n。1 <= n <= 10000。

输出

所有小于等于n的素数对。每对素数对输出一行，中间用单个空格隔开。若没有找到任何素数对，输出empty。

样例输入

```
100
```

样例输出

```
3 5
5 7
11 13
17 19
29 31
41 43
59 61
71 73
```

Ac代码，如果只看奇数可以更快

```c++
#include <bits/stdc++.h>
using namespace std;
int ifOK(int x)
{
    for(int i = 2; i*i<=x; i++)
    {
        if(x % i == 0)
        {
            return 0;
        }
    }
    return 1;
}
int main(void)
{
    int n;
    cin >> n;
    if(n <= 3)
    {
        printf("empty");
        return 0;
    }
    int OK = 0;
    for(int i = 3; i<=n; i+=2)
    {
        if(ifOK(i))
        {
            if(i+2<=n&&ifOK(i+2))
            {
                printf("%d %d\n",i,i+2);
                OK = 1;
            }
        }

    }
    if(!OK)
    {
        printf("empty");
    }
    return 0;
}
```

