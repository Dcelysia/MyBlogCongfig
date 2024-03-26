---
title: NOI1.11/编程基础之二分查找(10道题)
toc: true
date: 
cover: https://web-dcelysia-tlias.oss-cn-hangzhou.aliyuncs.com/myBlog/%E6%98%8E%E6%97%A5%E6%96%B9%E8%88%9F%E5%B0%8F%E4%BA%BA4.jpg
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



# NOI1.11/编程基础之二分查找(10道题)

发现NOI很少java的题解，所以我把我能做出来的题目用java写出来~~如果可以的话~~ ，其中也会有cpp的代码，但其实思路是一样的，不管用什么语言能把大致的算法写对都能过~~除非要很快，那只能用cpp~~ 如果你不擅长java或者cpp，可以留意看循环等能看懂的，虽然输入输出等都有很强的语言风格，但是算法的实现和逻辑都是一样的，尽量看你能看懂的，当然你也要对java的ArrayList等和cpp的stl有一点点的了解（本人也是蒟蒻一枚，题解并不是最优解，写的不是最漂亮的)

## 01:查找最接近的元素

**描述**

在一个非降序列中，查找与给定值最接近的元素。

**输入**

第一行包含一个整数n，为非降序列长度。1 <= n <= 100000。
第二行包含n个整数，为非降序列各元素。所有元素的大小均在0-1,000,000,000之间。
第三行包含一个整数m，为要询问的给定值个数。1 <= m <= 10000。
接下来m行，每行一个整数，为要询问最接近元素的给定值。所有给定值的大小均在0-1,000,000,000之间。

**输出**

m行，每行一个整数，为最接近相应给定值的元素值，保持输入顺序。若有多个值满足条件，输出最小的一个。

样例输入

```
3
2 5 8
2
10
5
```

样例输出

```
8
5
```

数据量很大，所以用二分是最好的，~~当然这10道有9道都是二分~~，可以用二分找到第一个大于等于输入值，这时候还要把左右两边跟输入值相减，那个大要哪个

AC代码如下:

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main{
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String tmp = br.readLine();
        int n = Integer.parseInt(tmp);
        tmp = br.readLine();
        int[] arr = new int[n];
        String[] nums = tmp.split(" ");
        for (int i = 0; i < n; i++) {
            arr[i] = Integer.parseInt(nums[i]);
        }
        int L = Integer.parseInt(br.readLine());
        for (int i = 0; i < L; i++) {
            int find = Integer.parseInt(br.readLine());
            int left = 0;
            int right = n - 1;
            while (left <= right) {
                int mid = (left + right) >>> 1;
                if (arr[mid] >= find) {
                    right = mid - 1;
                } else {
                    left = mid + 1;
                }
            }
            if(left >= n){
                left = n-1;
                System.out.println(arr[left]);
            }
            else if(left <=0){
                left=  0;
                System.out.println(arr[left]);
            }else{
                if(arr[left]-find > Math.abs(arr[left-1]-find)){
                    System.out.println(arr[left-1]);
                }else if(arr[left]-find < Math.abs(arr[left-1]-find)){
                    System.out.println(arr[left]);
                }else{
                    System.out.println(arr[left-1]);
                }
            }
            
        }
    }
}
```



## 02:二分法求函数的零点

- 总时间限制: 

  1000ms

- 内存限制: 

  65536kB

- 描述

  有函数：f(x) = x5 - 15 * x4+ 85 * x3- 225 * x2+ 274 * x - 121已知 f(1.5) > 0 , f(2.4) < 0 且方程 f(x) = 0 在区间 [1.5,2.4] 有且只有一个根，请用二分法求出该根。

- 输入

  无。

- 输出

  该方程在区间[1.5,2.4]中的根。要求四舍五入到小数点后6位。

- 样例输入

  `无`

- 样例输出

  `不提供`

  也是用二分求出来答案就行，精度要求小数点6位，那你while循环right-left就要小于1e-6(理论上)，这个题可以记住，下面有个差不多的，算是二分的一种常见方法

  AC代码如下:

  ```java
  import java.io.IOException;
  
  public class Main{
      static double zero = 0.000000;
      public static double f(double x) {
          double tmp = x * x * x * x * x - 15 * x * x * x * x + 85 * x * x * x - 225 * x * x + 274 * x - 121;
          return tmp;
      }
      public static void main(String[] args) throws IOException {
          double left = 1.5;
          double right = 2.4;
          double mid = 0;
          while(right - left >= 1e-10){
              mid = (left+right)/2.0;
              if(f(mid) >0){
                  left = mid;
              }else{
                  right = mid;
              }
          }
          System.out.printf("%.6f",mid);
      }
  }
  ```

## 03:矩形分割

- 总时间限制: 

  1000ms

- 内存限制: 

  65536kB

- 描述

  平面上有一个大矩形，其左下角坐标（0，0），右上角坐标（R,R)。大矩形内部包含一些小矩形，小矩形都平行于坐标轴且互不重叠。所有矩形的顶点都是整点。要求画一根平行于y轴的直线x=k（k是整数) ，使得这些小矩形落在直线左边的面积必须大于等于落在右边的面积，且两边面积之差最小。并且，要使得大矩形在直线左边的的面积尽可能大。注意：若直线穿过一个小矩形，将会把它切成两个部分，分属左右两侧。

- 输入

  第一行是整数R，表示大矩形的右上角坐标是(R,R) (1 <= R <= 1,000,000)。 接下来的一行是整数N,表示一共有N个小矩形(0 < N <= 10000)。 再接下来有N 行。每行有4个整数，L,T, W 和 H, 表示有一个小矩形的左上角坐标是(L,T),宽度是W，高度是H (0<=L,T <= R, 0 < W,H <= R). 小矩形不会有位于大矩形之外的部分。

- 输出

  输出整数n，表示答案应该是直线 x=n。 如果必要的话，x=R也可以是答案。

- 样例输入

  `1000 2 1 1 2 1 5 1 2 1`

- 样例输出

  `5`

这道题其实可以记住，如果求面积，而且是整数的画，你只用记住宽高中的一个，然后for循环遍历就行，就跟数学把一个面积拆成无数条线一样，不过如果你是整数的话其实都是宽为1，高为你记录的数组就行了

我可能说的有点抽象，这道题的思路就是只用记录x点的高度就行了，看AC代码

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;


public class Main{
    static int n;
    static long[]  hs;
    static long output = Long.MAX_VALUE;
    static int out = -1;
    static int R;
    public static boolean ifOK(int X) {
        long leftS = 0;
        long rightS = 0;
        for (int i = 0; i < X; i++) {
            leftS += hs[i];
        }
        for (int i = X; i <= R; i++) {
            rightS += hs[i];
        }
        if(leftS >=rightS){
            if(leftS-rightS<=output){
                output = leftS - rightS;
                out = X;
            }
            return true;
        }
        return false;
    }

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        R = Integer.parseInt(br.readLine());
        n = Integer.parseInt(br.readLine());
        if (n == 0) {
            System.out.println(R);
            System.exit(0);
        }
        hs = new long[R+1];
        for (int i = 0; i < n; i++) {
            String[] strs = br.readLine().split(" ");
            int xs = Integer.parseInt(strs[0]);
            int ws = Integer.parseInt(strs[2]);
            //上面都是输入数据，看不懂也没关系
            for(int j = xs;j<xs+ws;j++){
                //这里关键，就是把从x开头到x+ws(结尾不用)的结尾这一段全部加上高度，之后求面积只需要加高度就行了
                //觉得抽象的可以想象这是一个长度是1高度是hs[j]的面积相加，你就知道为什么结尾x+ws不用了
                hs[j] += Integer.parseInt(strs[3]);
            }
        }
        
        int left = 0;
        int right = R;
        //正常的二分
        while (left <= right) {
            int mid = (left + right) >>> 1;
            if (ifOK(mid)) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        //我喜欢把赋值在函数里面就弄好了，这样出来直接用out就不会因为二分条件改变而乱
        //如果这里没高，可以继续把线往右边移
        while(hs[out] == 0&&out < R){
            out++;
        }
        System.out.println(out);
    }
}
```

## 04:网线主管



描述

仙境的居民们决定举办一场程序设计区域赛。裁判委员会完全由自愿组成，他们承诺要组织一次史上最公正的比赛。他们决定将选手的电脑用星形拓扑结构连接在一起，即将它们全部连到一个单一的中心服务器。为了组织这个完全公正的比赛，裁判委员会主席提出要将所有选手的电脑等距离地围绕在服务器周围放置。

为购买网线，裁判委员会联系了当地的一个网络解决方案提供商，要求能够提供一定数量的等长网线。裁判委员会希望网线越长越好，这样选手们之间的距离可以尽可能远一些。

该公司的网线主管承接了这个任务。他知道库存中每条网线的长度（精确到厘米），并且只要告诉他所需的网线长度（精确到厘米），他都能够完成对网线的切割工作。但是，这次，所需的网线长度并不知道，这让网线主管不知所措。

你需要编写一个程序，帮助网线主管确定一个最长的网线长度，并且按此长度对库存中的网线进行切割，能够得到指定数量的网线。

输入

第一行包含两个整数N和K，以单个空格隔开。N（1 <= N <= 10000）是库存中的网线数，K（1 <= K <= 10000）是需要的网线数量。
接下来N行，每行一个数，为库存中每条网线的长度（单位：米）。所有网线的长度至少1m，至多100km。输入中的所有长度都精确到厘米，即保留到小数点后两位。

输出

网线主管能够从库存的网线中切出指定数量的网线的最长长度（单位：米）。必须精确到厘米，即保留到小数点后两位。
若无法得到长度至少为1cm的指定数量的网线，则必须输出“0.00”（不包含引号）。

样例输入

```
4 11
8.02
7.43
4.57
5.39
```

样例输出

```
2.00
```

这个其实更简单，你可以用把输入的数据全部乘2然后再算就行了，因为这个它说了最多小数点后2位

然后就是普通二分，就直接每条线都和mid相除，除出来的结果相加就是能分出来的电线的数量，再判断够不够就行了，不够就降低长度，够就增加长度

AC代码如下:

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main{
    static int n;
    static int num;
    static int[] lens;
    static int out = 0;

    public static boolean ifOK(int x) {
        int sum = 0;
        for (int i = 0; i < n; i++) {
            sum += lens[i] / x;
        }
        if (sum >= num) {
            out = x;
            return true;
        }
        return false;
    }

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String str = br.readLine();
        n = Integer.parseInt(str.split(" ")[0]);
        num = Integer.parseInt(str.split(" ")[1]);
        lens = new int[n];
        int max = Integer.MIN_VALUE;
        for (int i = 0; i < n; i++) {
            double tmp = Double.parseDouble(br.readLine());
            lens[i] = (int) (tmp * 100);
            //找最大值，这里上面都是输入数据的，看不懂也没关系
            max = Math.max(max, lens[i]);
        }
        int left = 1;
        int right = max;
        while (left <= right) {
            int mid = (left + right) >>> 1;
            if (ifOK(mid)) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        if(out == 0){
            System.out.println("0.00");
        }else{
            System.out.printf("%.2f",(double)out/100);
        }

    }
}
```

## 05:派

- 总时间限制: 

  1000ms

- 内存限制: 

  65536kB

- 描述

  我的生日要到了！根据习俗，我需要将一些派分给大家。我有N个不同口味、不同大小的派。有F个朋友会来参加我的派对，每个人会拿到一块派（必须一个派的一块，不能由几个派的小块拼成；可以是一整个派）。我的朋友们都特别小气，如果有人拿到更大的一块，就会开始抱怨。因此所有人拿到的派是同样大小的（但不需要是同样形状的），虽然这样有些派会被浪费，但总比搞砸整个派对好。当然，我也要给自己留一块，而这一块也要和其他人的同样大小。  请问我们每个人拿到的派最大是多少？每个派都是一个高为1，半径不等的圆柱体。 

- 输入

  第一行包含两个正整数N和F，1 ≤ N, F ≤ 10 000，表示派的数量和朋友的数量。 第二行包含N个1到10000之间的整数，表示每个派的半径。

- 输出

  输出每个人能得到的最大的派的体积，精确到小数点后三位。

- 样例输入

  `3 3 4 3 3`

- 样例输出

  `25.133`

这个跟那个网线也差不多，就是变成了有小数参与的

这里while循环里left+多少和right减多少其实~~看心情~~，我这里可以稍大一点,但再小一点就错了

AC代码如下:

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main{
    static int friendNum;
    static int n;
    static double[] cakes;
    static double output = 0;

    public static boolean ifOK(double x) {
        int sum = 0;
        for (int i = 0; i < n; i++) {
            sum += cakes[i] / x;
        }
        if (sum >= friendNum) {
            output = x;
            return true;
        }
        return false;
    }

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String str = br.readLine();
        n = Integer.parseInt(str.split(" ")[0]);
        friendNum = Integer.parseInt(str.split(" ")[1]);
        friendNum++;
        String[] strs = br.readLine().split(" ");
        cakes = new double[n];
        double right = Long.MIN_VALUE;
        for (int i = 0; i < n; i++) {
            int tmp = Integer.parseInt(strs[i]);
            cakes[i] = tmp * Math.PI * tmp;
            right = Math.max(right, cakes[i]);
        }
        double left = 0;
        while (left <= right) {
            double mid = (left + right) / 2;
            if (ifOK(mid)) {
                left = mid + 0.00001;
            } else {
                right = mid - 0.00001;
            }
        }
        System.out.printf("%.3f", output);
    }
}
```

## 06:月度开销

描述

农夫约翰是一个精明的会计师。他意识到自己可能没有足够的钱来维持农场的运转了。他计算出并记录下了接下来 *N* (1 ≤ *N* ≤ 100,000) 天里每天需要的开销。

约翰打算为连续的*M* (1 ≤ *M* ≤ *N*) 个财政周期创建预算案，他把一个财政周期命名为fajo月。每个fajo月包含一天或连续的多天，每天被恰好包含在一个fajo月里。

约翰的目标是合理安排每个fajo月包含的天数，使得开销最多的fajo月的开销尽可能少。



输入

第一行包含两个整数N,M，用单个空格隔开。
接下来N行，每行包含一个1到10000之间的整数，按顺序给出接下来N天里每天的开销。

输出

一个整数，即最大月度开销的最小值。

样例输入

```
7 5
100
400
300
100
500
101
400
```

样例输出

```
500
```

提示

若约翰将前两天作为一个月，第三、四两天作为一个月，最后三天每天作为一个月，则最大月度开销为500。其他任何分配方案都会比这个值更大。

如果遍历月份的之后这个fajo月里大于mid，那就要多分一个fajo月，如果最后分的月小于等于M就是可以的

如果遍历到花的钱大于mid的月，那这个mid一定实现不了

其实代码都差不多

AC代码:

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main{
    static int n, m;
    static int[] a;
    static long output;

    public static boolean ifOK(long x) {
        long sum = 0;
        int num = 1;
        for (int i = 0; i < n; i++) {
            if (sum + a[i] > x) {
                sum = 0;
                num++;
            }
            if (a[i] > x) {
                return false;
            }
            sum += a[i];
            if (num > m) {
                return false;
            }
        }
        output = x;
        return true;
    }

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String tmp = br.readLine();
        n = Integer.parseInt(tmp.split(" ")[0]);
        m = Integer.parseInt(tmp.split(" ")[1]);
        a = new int[n];
        long max = Long.MIN_VALUE;
        long sum = 0;
        for (int i = 0; i < n; i++) {
            a[i] = Integer.parseInt(br.readLine());
            max = Math.max(max, a[i]);
            sum += a[i];
        }
        long left = max;
        long right = sum;
        while (left <= right) {
            long mid = (left + right) >>> 1;
            if (ifOK(mid)) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        System.out.println(output);
    }
}
```

## 07:和为给定数



- 总时间限制: 

  1000ms

- 内存限制: 

  65536kB

- 描述

  给出若干个整数，询问其中是否有一对数的和等于给定的数。 

- 输入

  共三行： 第一行是整数n(0 < n <= 100,000)，表示有n个整数。 第二行是n个整数。整数的范围是在0到10^8之间。 第三行是一个整数m（0 <= m <= 2^30)，表示需要得到的和。

- 输出

  若存在和为m的数对，输出两个整数，小的在前，大的在后，中间用单个空格隔开。若有多个数对满足条件，选择数对中较小的数更小的。若找不到符合要求的数对，输出一行No。

- 样例输入

  `4 2 5 1 4 6`

- 样例输出

  `1 5`

这道题真是，我用BufferReader快输最后一个一直过不了，然后用了Scanner才OK了，对于double和long感觉快输都不是特别靠谱(

先排序，然后用和减当前数据的值，往后找看看能不能找到就行了

AC代码:

```java
import java.io.IOException;
import java.util.Arrays;
import java.util.Scanner;

public class Main{
 public static void main(String[] args) throws IOException {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        Long[] nums = new Long[n];
        for (int i = 0; i < n; i++) {
            nums[i] = sc.nextLong();
        }
        long sum = sc.nextLong();
        Arrays.sort(nums);
        long out1 = -1;
        long out2 = -1;
        for (int i = 0; i < n; i++) {
            if (nums[i] >= sum) {
                break;
            }
            long tmp = sum - nums[i];
            int left = i + 1;
            int right = n - 1;
            int index = -1;
            while (left <= right) {
                int mid = (left + right) >>> 1;
                if (nums[mid] > tmp)
                    right = mid - 1;
                else if (nums[mid] < tmp)
                    left = mid + 1;
                else {
                    index = mid;
                    break;
                }
            }
            if (index != -1) {
                out1 = nums[i];
                out2 = nums[index];
                break;
            }
        }
        if (out1 == -1) {
            System.out.println("No");
        } else {
            System.out.println(out1 + " " + out2);
        }
    }
}
```

## 08:不重复地输出数

- 总时间限制: 

  1000ms

- 内存限制: 

  65536kB

- 描述

  输入n个数，从小到大将它们输出，重复的数只输出一次。保证不同的数不超过500个。

- 输入

  第一行是一个整数n。1 <= n <= 100000。 之后n行，每行一个整数。整数大小在int范围内。

- 输出

  一行，从小到大不重复地输出这些数，相邻两个数之间用单个空格隔开。

- 样例输入

  `5 2 4 4 5 1`

- 样例输出

  `1 2 4 5`

不需要二分，java可以用TreeSet,不会用的同学也可以先排序(一定要排序)然后判断strs[i] != strs[i-1]就行了，这样就可以避免输出相同的数据

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.TreeSet;

public class Main{
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        TreeSet<Integer> ts = new TreeSet<>();
        int n = Integer.parseInt(br.readLine());
        String[] strs = br.readLine().split(" ");
        for (int i = 0; i < n; i++) {
            ts.add(Integer.parseInt(strs[i]));
        }
        int tmp = ts.size();
        for (int i = 0; i < tmp; i++) {
            System.out.print(ts.pollFirst() + " ");
        }
    }
}
```

## 09:膨胀的木棍 

总时间限制: 

1000ms

内存限制: 

65536kB      [有图可以看一下图长什么样](http://noi.openjudge.cn/ch0111/09/)

描述

当长度为L的一根细木棍的温度升高n度，它会膨胀到新的长度L'=(1+n*C)*L，其中C是热膨胀系数。当一根细木棍被嵌在两堵墙之间被加热，它将膨胀形成弓形的弧，而这个弓形的弦恰好是未加热前木棍的原始位置。你的任务是计算木棍中心的偏移距离。

输入

三个非负实数：木棍初始长度（单位：毫米），温度变化（单位：度），以及材料的热膨胀系数。
保证木棍不会膨胀到超过原始长度的1.5倍。

输出

木棍中心的偏移距离（单位：毫米），保留到小数点后第三位。

样例输入

```
1000 100 0.0001
```

样例输出

```
61.329
```

这个是二分那个圆心角，然后通过数学去解决就行了，cpp和java都有三角甚至反三角函数用的

数学关系看我那个式子，或者自己想一下能出来

这个和函数零点有点相似

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.TreeSet;

public class Main{
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String tmp = br.readLine();
        double L = Double.valueOf(tmp.split(" ")[0]);
        double n = Double.valueOf(tmp.split(" ")[1]);
        double c = Double.valueOf(tmp.split(" ")[2]);
        double LL = (1+n*c)*L;
        double left = 0;
        double right = Math.PI;
        double mid = 0;
        while(right - left >= 1e-12)
        {
            mid = (left + right) / 2;
            if(L*mid/(2*Math.sin(mid/2)) < LL)
               left = mid;
            else
               right = mid;
        }                         
        System.out.printf("%.3f", LL/mid*(1-Math.cos(mid/2)));
    }
}
```

## 10:河中跳房子

- 总时间限制: 

  1000ms

- 内存限制: 

  65536kB

- 描述

  每年奶牛们都要举办各种特殊版本的跳房子比赛，包括在河里从一个岩石跳到另一个岩石。这项激动人心的活动在一条长长的笔直河道中进行，在起点和离起点L远 (1 ≤ *L*≤ 1,000,000,000) 的终点处均有一个岩石。在起点和终点之间，有*N* (0 ≤ *N* ≤ 50,000) 个岩石，每个岩石与起点的距离分别为*Di (0 < \*Di\* < \*L*)。*在比赛过程中，奶牛轮流从起点出发，尝试到达终点，每一步只能从一个岩石跳到另一个岩石。当然，实力不济的奶牛是没有办法完成目标的。农夫约翰为他的奶牛们感到自豪并且年年都观看了这项比赛。但随着时间的推移，看着其他农夫的胆小奶牛们在相距很近的岩石之间缓慢前行，他感到非常厌烦。他计划移走一些岩石，使得从起点到终点的过程中，最短的跳跃距离最长。他可以移走除起点和终点外的至多*M* (0 ≤ *M* ≤ *N*) 个岩石。请帮助约翰确定移走这些岩石后，最长可能的最短跳跃距离是多少？ 

- 输入

  第一行包含三个整数L, N, M，相邻两个整数之间用单个空格隔开。 接下来N行，每行一个整数，表示每个岩石与起点的距离。岩石按与起点距离从近到远给出，且不会有两个岩石出现在同一个位置。

- 输出

  一个整数，最长可能的最短跳跃距离。

- 样例输入

  `25 5 2 2 11 14 17 21`

- 样例输出

  `4`

- 提示

  在移除位于2和14的两个岩石之后，最短跳跃距离为4（从17到21或从21到25）。

这道题我在洛谷也见过，其实就是二分最短跳跃距离，如果两个岩石之间的距离比最短跳跃距离小，那就要移走这个石头，然后移动步数++，最后看移动步数有没有超就行了,判断里的now和index用的很妙，now就是上一个石头的位置，index就是现在的石头的位置，中间可能会有不要的石头的，这样可以解决石头被移走了下一个石头要和上上个石头相减的问题

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main{
    static int len;
    static int num;
    static int[] stones;
    static int output = 0;
    public static boolean ifOK(int mid) {
        int index = 0;
        int tot = 0;
        int now = 0;
        while (index < len + 1) {
            index++;

            if (stones[index] - stones[now] < mid) {
                tot++;
            } else {
                now = index;
            }
            if (tot > num) {
                return false;
            }
        }
        output = mid;
        return true;
    }

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String strTmp = br.readLine();
        int far = Integer.parseInt(strTmp.split(" ")[0]);
        len = Integer.parseInt(strTmp.split(" ")[1]);
        num = Integer.parseInt(strTmp.split(" ")[2]);
        stones = new int[len + 2];
        stones[len + 1] = far;
        stones[0] = 0;
        
        for (int i = 1; i <= len; i++) {
            String numStone = br.readLine();
            stones[i] = Integer.parseInt(numStone);
        }
        int left = 0;
        int right = far;
        while (left <= right) {
            int mid = (left + right) >>> 1;
            if (ifOK(mid)) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        System.out.println(output);
    }
}
```

[这道题在洛谷也有](https://www.luogu.com.cn/problem/P2678)

## 致谢

感谢你的观看，如果有讲错的地方大佬也可以指出