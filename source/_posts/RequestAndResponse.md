---
title: RequestAndResponse
toc: true
date: 
cover: /img/明日方舟小人1.jpg
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



# Request

## 概念

Request是用来存放请求数据的一个对象

## Request体系结构

**ServletRequest** 是由java提供的请求对象的接口

**HttpServletRequest** 是由java提供的封装了http协议的请求对象的接口

**RequestFacade** 是Tomcat定义的实现类

如果你的类是继承的Servlet接口，那你的service()里用的就是ServletRequest

如果继承的是HttpServlet，那你的doGet()和doPost()里面就是HttpServletRequest

## Request获取数据

### 请求方法为get

```java
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //获取访问方法(get/post)GET
        String method = req.getMethod();
        System.out.println(method);
        //获取项目访问路径/MavenWeb
        String contextPath = req.getContextPath();
        System.out.println(contextPath);
        //获取url(统一资源定位符)http://localhost:8080/MavenWeb/req1
        StringBuffer url = req.getRequestURL();
        System.out.println(url.toString());
        //获取uri(统一资源标志符)/MavenWeb/req1
        String uri = req.getRequestURI();
        System.out.println(uri);
        //获取get出来的键值对的请求体username=zhangsan&password=123
        String queryString = req.getQueryString();
        System.out.println(queryString);
        //-----------------------------------------
        //request获取请求头Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/121.0.0.0 Safari/537.36 Edg/121.0.0.0
        String userAgent = req.getHeader("user-agent");//请求头的名称
        System.out.println(userAgent);
    }

```

### 请求方法为post

```java
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //用输入流的方式读取请求体username=lisi&password=1234
        BufferedReader reader = req.getReader();
        String line = reader.readLine();
        System.out.println(line);
    }
```

以上的方法有两个弊端，其一是post和get可能只有读取数据不同，大多数代码逻辑都是一样的，其二就是获取出来的键值对还有自己拆分，很麻烦。

## 统一获取数据

统一获取数据后get()和post()方法都可以通用，且数据都被存入了一个map集合里面，方便访问

```java
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //获取数据的map集合
        /*username:123
        password:123
        hobby:1 2*/
        Map<String, String[]> map = req.getParameterMap();
        for (String s : map.keySet()) {
            System.out.print(s + ":");
            String[] keys = map.get(s);
            for (String key : keys) {
                System.out.print(key + " ");
            }
            System.out.println();
        }
        //用键名去获取一个数据 123
        String username = req.getParameter("username");
        System.out.println(username);
        //用键名去获取多个数据
        //1
        //2
        String[] hobbies = req.getParameterValues("hobby");
        for (String hobby : hobbies) {
            System.out.println(hobby);
        }

    }
 @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        Map<String, String[]> map = req.getParameterMap();
        for (String s : map.keySet()) {
            System.out.print(s + ":");
            String[] keys = map.get(s);
            for (String key : keys) {
                System.out.print(key + " ");
            }
            System.out.println();
        }
        //用键名去获取一个数据 123
        String username = req.getParameter("username");
        System.out.println(username);
        //用键名去获取多个数据
        //1
        //2
        String[] hobbies = req.getParameterValues("hobby");
        for (String hobby : hobbies) {
            System.out.println(hobby);
        }

}
```

### 统一获取数据的弊端

如果遇到中文可能出现乱码现象

原因: 这是因为浏览器返回来的数据是用UTF-8进行URL编译的，而Tomcat使用ISO_8859_1进行URL反编译

如果是post方法，因为原理是用字符流输入，所以可以改变字符流的编码规范用UTF-8

```java
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.setCharacterEncoding("utf-8");
        System.out.println(req.getParameter("username"));
}
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req,resp);
}
```

但是这样解决不了get方法的问题

所以也可以把读取到的数据先转化成二进制，然后再转化成UTF-8，这样get和post方法都不会出现中文乱码

```java
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String username = new String(req.getParameter("username").getBytes(StandardCharsets.ISO_8859_1),StandardCharsets.UTF_8);
        System.out.println(username);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req,resp);
    }
```

## Request转发

可以进行服务器内部资源的跳转，例如资源A可以访问到资源B

实现方式

```java
req.getRequestDispatcher("资源B路径").forward(req,resp);
```

资源A

````java
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.getRequestDispatcher("/req5").forward(req,resp);
        System.out.println("req4");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req,resp);
    }
````

资源B

```java
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("req5");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req,resp);
    }
```

打印结果是先req5后req4

两个资源可以把数据存到request域中，这样就可以做到资源共享

```java
req.setAttribute(String name,Object); 存到request域中
```

```java
req.getAttribute(String name); 通过名字访问数据
```

```java
req.removeAttribute(String name); 通过名字删除数据
```



```java
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //存数据
        req.setAttribute("username","张三");
        req.getRequestDispatcher("/req5").forward(req,resp);
        System.out.println("req4");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req,resp);
    }
```

```java
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("req5");
        //访问数据
        System.out.println(req.getAttribute("username"));
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req,resp);
    }
结果是
req5
张三
req4
```

**特点** ：浏览器的地址不会发生变化

​             只能转发至服务器内部资源

​             一次请求，可以在转发的资源间使用request共享数据

# Response

## Response结构体系

跟Request一个模子里刻出来

**ServletResponse** 是由java提供的请求对象的接口

**HttpServletResponse** 是由java提供的封装了http协议的请求对象的接口

**ResponseFacade** 是Tomcat定义的实现类

## 设置响应数据

Response主要有响应行，响应头和响应体

**响应行**：

```java
void setStatus(int sc)//用来设置响应状态码
```

**响应头**：

```java
void setHeader(String name,String value)//设置响应头键值对
```

**响应体**:

```java
PrintWriter getWriter();//获取字符输出流 
```

````java
ServletOutputStream getOutputStream(); //获取字节输入流
````

## Response的重定向

就是资源A如果不能满足要求，则把响应状态码改成302,响应头的location改成资源B的路径，就可以范围到资源B

资源A

```java
@WebServlet("/resp1")
public class ResponseDemo1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("resp1");
        resp.setStatus(302);//设置响应状态码 
        resp.setHeader("location","/MavenWeb/resp2");//设置响应 
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req,resp);
    }
}
简化版
@WebServlet("/resp1")
public class ResponseDemo1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("resp1");
        String path = req.getContextPath();
        resp.sendRedirect(path+"/resp2");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req,resp);
    }
}

```

资源B

```java
@WebServlet("/resp2")
public class ResponseDemo2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("resp2");
    }
}
```

最终访问resp1后控制台会打印resp1和resp2

**Response重定向特点：**

1. 即使不在服务器内部的资源也可以访问
2. 访问后浏览器地址会改变
3. 不像转发一样会有request共享数据

## Response响应字符输出

用resp.getWriter()获取字符输出流来输出响应体数据

```java
resp.setContentType("text/html;charset=utf-8");//用来设置响应文本类型和输出编码规则
```

```java
resp.setHeader("content-type","text/html"); //设置响应头也可以设置响应文本类型，但是不能设置输出编码规则，如果是Tomcat8以下的版本输出中文会出现乱码
```



```java
@WebServlet("/resp3")
public class ResponseDemo3 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("resp3");
        resp.setContentType("text/html;charset=utf-8");
        PrintWriter writer = resp.getWriter();
        writer.write("你好");
        writer.write("<h1>aaaa</h1>");
    }
}
```

## Response响应字节输出

```java
@WebServlet("/resp4")
public class ResponseDemo4 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("resp4");
        //获取字节输入流
        FileInputStream fis = new FileInputStream("D:\\桌面\\壁纸\\void棺材胡桃4k.jpg");
        //获取响应字节输出流
        ServletOutputStream os = resp.getOutputStream();
        byte[] buff = new byte[1024*1024*5];
        int len ;
        while((len = fis.read(buff)) != -1){
            os.write(buff,0,len);
        }
    }
}
```

这样就可以在网站里看到输出的图片了

当然，上面的输入输出过于繁琐，所以可以用IOUtils工具类实现

先在pom.xml中导入坐标

```xml
       <dependency>
            <groupId>commons-io</groupId>
            <artifactId>commons-io</artifactId>
            <version>2.6</version>
        </dependency>
```

·然后

```java
@WebServlet("/resp4")
public class ResponseDemo4 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("resp4");
        //获取字节输入流
        FileInputStream fis = new FileInputStream("D:\\桌面\\壁纸\\void棺材胡桃4k.jpg");
        //获取响应字节输出流
        ServletOutputStream os = resp.getOutputStream();
        
        IOUtils.copy(fis,os);

    }
}
```

就可以了！！
