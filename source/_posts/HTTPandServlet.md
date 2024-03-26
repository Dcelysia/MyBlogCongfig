---
title: HTTPandServlet
toc: true
date: 
cover: http://say9y2uwt.hn-bkt.clouddn.com/myblogPicture/%E6%98%8E%E6%97%A5%E6%96%B9%E8%88%9F%E5%B0%8F%E4%BA%BA2.jpg
categories: 个人笔记
description: 想不出来介绍了
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



# HTTP

## 概念

全称是超文本传输协议，规定了服务器和浏览器之间的请求和响应的规则（是B/S框架)

特点：

1.  是基于tcp协议的，连接安全
2. 对于一次请求只会有一次响应
3. 多次请求响应之间数据不互通，都是独立的，缺点就是多次请求的数据不能互通，优点是速度快

## HTTP的请求格式

分为三个部分

1. **请求行** 在请求数据的第一行，是用来存放请求方式，请求资源路径，其中还会包括协议版本

2. **请求头** 有key: value构成(这里有空格) 包括一些请求的特点，例如

   Host :请求的主机名

   User-Agent: 浏览器版本

   Accept: 表示浏览器能接收的资源类型

   Accept-language: 表示浏览器偏好的语言

   Accept-Encoding: 表示浏览器可以支持的压缩类型

   以上这些都是给服务器用的

       3. **请求体** 用来存放请求参数，如果是post的话请求参数会放到请求体力，如果是get的话请求参数会放到请求行里，get能存放的请求体式有大小限制的，而post没有

## HTTP的响应格式

跟请求格式差不多分为三个部分

1. **响应行** 会有响应协议版本，响应状态码和响应状态描述 其中响应状态码大致分为

   1xx 表示请求中，请求读写未完成，客户端应该继续请求

   2xx 表示接受

   3xx 表示重定向，重定向到其他地方，要客户端再发送一个请求以完成整个过程

   4xx 出现错误，错在客户端

   5xx 出现错误，错在服务端

   2. 响应头 有key: value构成(这里有空格) 包括一些响应的特点，例如

      Content-type: 表示响应的内容

      Content-length: 表示响应的长度

2. 响应体  用来存放响应数据，就比如html文件

# Servlet

## 概念

servlet是用来管理web资源里的动态资源，每个人在访问一个资源的时候可能展示出来的都不一样，例如头像介绍什么的，所以需要用动态资源对每个人展示出来的资源进行管理

## 使用

用maven导入servlet

```xml

    <dependencies>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.1.0</version>
            <scope>provided</scope>
        </dependency>
    </dependencies>

```

然后再在java中创建Servlet接口的实现类并重写Servlet中的service  **@WebServlet("/demo1")**是用来规定浏览器访问路径的

```java
import javax.servlet.*;
import javax.servlet.annotation.WebServlet;
import java.io.IOException;
@WebServlet("/demo1")
public class ServletDemo1 implements Servlet {
    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        //如果访问到Servlet就会在控制台打印Hello Servlet
        System.out.println("Hello Servlet!");
    }

    @Override
    public void init(ServletConfig servletConfig) throws ServletException {

    }

    @Override
    public ServletConfig getServletConfig() {
        return null;
    }
    @Override
    public String getServletInfo() {
        return null;
    }

    @Override
    public void destroy() {

    }
}

```

## Servlet的工作流程和生命周期

Servlet会有web服务器创建，service方法也会由web服务器进行执行，其生命周期由容器来管理

1. **加载和实例** : 在默认不设置的情况下，第一次访问Servlet的时候会由容器创建Servlet对象
2. **初始化** : 在给Servlet实例化的时候，会调用Init() ，这个方法只会调用一次
3. **请求处理** : 每次请求Servlet的时候，会调用service()方法对请求进行处理
4. **服务终止** : 当需要释放内存或者关闭容器的时候，容器会调用Servlet的destroy()对这个实例进行一个销毁，随后这个实例被java垃圾收集器所回收

## HttpServlet

HttpServlet是Servlet的一个子类，可以让我们很方便的写关于Servlet的service，主要用doPost()和doGet()去规定当用get访问和post访问时候不同的行为

```java

@WebServlet("/demo2")
public class ServletDemo2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("get....");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("post....");
    }
}

```

如果用post访问，就会执行post方法，例如用表单把方法弄成method = "post" 然后action = "路径"

<input type = "submit"\>就可以用post访问了

get访问直接在网页上打出地址就可以了

### HttpServlet伪实现

这里为了方便观看把其他Servlet函数给删掉了，但实际上还是要写出来的

先用强转把servletRequest转成HttpServletRequest然后用getMethod()获得请求方法

之后调用不同的对应请求方法的函数

```java
public class MyHttpServlet implements Servlet {
    
    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        HttpServletRequest hs = (HttpServletRequest) servletRequest;
        String method = hs.getMethod();
        if(method.equals("GET")){
            doGET();
        }else if(method.equals("POST"))
        {
            doPost();
        }
    }
    protected void doPost() {
    }

    protected void doGET() {
    }
}
```

## urlPatterns

是用来存储url的一个字符串数组，里面可以写多个访问路径，例如

```java
@WebServlet(urlPatterns = {"/demo3","/demo333"})
```

访问/demo3和/demo333都可以访问到这个HttpServlet

### urlPattern的匹配规则

**精准匹配** 不用*号，在输入地址的时候要完全一样的配置 

```java
@WebServlet("/demo2")
public class ServletDemo2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("get....");
    }
}
```

**目录匹配**

```java
@WebServlet("/demo3/*")
public class ServletDemo4 extends MyHttpServlet {
    @Override
    protected void doGET() {
        System.out.println("get4");
    }
}
```

输入/demo3/aaa 或者 /demo3/bbb 等 都可以访问到

**扩展名匹配** 

 ```java
 @WebServlet("*.demo5")
 public class ServletDemo5 extends MyHttpServlet {
     @Override
     protected void doGET() {
         System.out.println("get5");
     }
 }
 ```

输入/aaa.demo5 或者 /bbb.demo5 等 都可以

**任意匹配** 

```java
@WebServlet("/")
public class ServletDemo7 extends MyHttpServlet {
    @Override
    protected void doGET() {
        System.out.println("get7");
    }
}
```

```java
@WebServlet("/*")
public class ServletDemo8 extends MyHttpServlet {
    @Override
    protected void doGET() {
        System.out.println("get8");
    }
}
```

这两种都是任意匹配

/*的优先级更高

注意：Tomcat的静态资源的url默认是/，如果你写动态资源的任意匹配，就可能访问不到静态资源了

**匹配规则的优先级: 精准匹配 > 目录匹配 > 扩展名匹配 > 任意匹配  **

## 用xml去配置Servlet

在web.xml去配置Servlet

```xml
  <servlet>
    <!-- 取名-->
    <servlet-name>demo9</servlet-name>
    <!-- 找到类的位置-->
    <servlet-class>com.yuanshen.web.ServletDemo9</servlet-class>
  </servlet>
  <servlet-mapping>
    <!--要配置的名字-->
    <servlet-name>demo9</servlet-name>
    <!--配置的url-->
    <url-pattern>/demo9</url-pattern>
  </servlet-mapping>
```