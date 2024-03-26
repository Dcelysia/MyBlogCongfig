---
title: IOCDIBean
toc: true
date: 
cover: https://web-dcelysia-tlias.oss-cn-hangzhou.aliyuncs.com/myBlog/%E6%98%8E%E6%97%A5%E6%96%B9%E8%88%9F%E5%B0%8F%E4%BA%BA3.jpg
description: 想不出来介绍了
categories: 个人笔记
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



# IOC/DI/Bean

IoC(控制反转)可以通过把类都交给IoC容器来做到充分解耦，调用对象可以从IoC容器中直接调用，在IoC容器里面初始化和创建的对象叫做Bean

在IoC中，不同的Bean可能会有继承关系，在IoC中也会对建立所需的依赖关系，这就是DI(依赖注入)

## IoC的实现(XML)

首先要在pom.xml导入spring的坐标

```xml
    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>6.1.3</version>
        </dependency>
    </dependencies>
```

之后在文件夹resources中创建配置Bean的xml文件，如果你前面正确导入坐标这时候右键会出一个     

**xml configuration file - >spring config**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

     <!--这里的配置类在下面定义了-->
    <bean id="User" class="com.yuanshen.pojo.User"></bean>
    <bean id="Student" class="com.yuanshen.pojo.Student"></bean>
</beans>
```

之后创建两个类准备塞IoC容器里面

```java
package com.yuanshen.pojo;

public class Student {
    private User user = new User();
    public void save(){
        System.out.println("Student..");
        user.save();
    }
}
```

```java
package com.yuanshen.pojo;

public class User {
    public void save(){
        System.out.println("User...");
    }
}

```

之后在主函数里面创建IoC容器，然后调用获取Bean并运行方法

```java
import com.yuanshen.pojo.Student;
import com.yuanshen.pojo.User;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Main {
    public static void main(String[] args) {
        //获取IoC容器通过XML文件
        ApplicationContext as = new ClassPathXmlApplicationContext("applicationContext.xml");
//        User user = (User) as.getBean("User");
//        user.save();
        Student st = (Student) as.getBean("Student");
        st.save();
    }
}
```

## DI的实现(XML)

上面在Student里还要new一个User，但是还可以通过DI实现Student和User的低耦合绑定

在xml文件中把Student和User用<property\>绑定

```java
public class Student {
    private User user;
    public void save() {
        System.out.println("Student..");
        user.save();
    }

    public void setUser(User user) {
        this.user = user;
    }
}

```

第一个name是 Student里的类名，第二个ref是赋予给user IoC容器中哪个Bean

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:util="http://www.springframework.org/schema/util"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/util https://www.springframework.org/schema/util/spring-util.xsd">
<!---->
    <bean id="User" class="com.yuanshen.pojo.User"></bean>

    <bean id="Student" class="com.yuanshen.pojo.Student" >
        <property name="user" ref="User"/>
    </bean>
</beans>
```

## Bean

### Bean别名

Bean的别名在<bean\>中写name值就是Bean的别名，别名可以用空格，','或者';'分离

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:util="http://www.springframework.org/schema/util"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/util https://www.springframework.org/schema/util/spring-util.xsd">
     <!--用空格分隔-->
    <bean id="User" class="com.yuanshen.pojo.User" name="user,user2"></bean>
    <!--用,分隔-->
    <bean id="Student" class="com.yuanshen.pojo.Student" name="student student2" >
        <property name="user" ref="User"/>
    </bean>
</beans>
```

## Bean的作用范围

```java
public class Main {
    public static void main(String[] args) {
        ApplicationContext as = new ClassPathXmlApplicationContext("applicationContext.xml");
        Student st1 = (Student) as.getBean("student");
        Student st2 = (Student) as.getBean("student");
        System.out.println(st1);
        System.out.println(st2);
    }
}
```

像这样通过Bean创建两个类，这时候两个类打印出来的地址值是完全一样的

```xml
    <bean id="Student" class="com.yuanshen.pojo.Student" name="student student2" scope="singleton">
        <property name="user" ref="User"/>
    </bean>
```

在<bean\>中设置scope可以调节是单例还是多例 **scope="singleton"**(默认)，就是单例，创建多个其实用的都是一个类

**scope="prototype"**是多例，这样每创建一个类都是开辟新的空间

## Bean实例化

### 构造方法

类似于这种绑定类本身并直接构造调用都是空参构造，如果没有空参构造就会报错

```java
public class Main {
    public static void main(String[] args) {
        ApplicationContext as = new ClassPathXmlApplicationContext("applicationContext.xml");
        User user = (User) as.getBean("User");
    }
}
```

```java
public class User {
    //设立一个空参构造
    public User(){
        System.out.println("construction~");
    }
    public void save(){
        System.out.println("User...");
    }
}
```

```xml
    <bean id="User" class="com.yuanshen.pojo.User" name="user,user2"></bean>
```

这样控制台就会输出一个 **"construction~"**

### 静态工厂

在创建bean的时候要写**factory-method**绑定构造函数就可以了

```xml
    <bean class="Factory.UserStaticFactory" factory-method="createUser" id="createUser"/>
```

```java
public class UserStaticFactory {
    public static User createUser(){
        return new User();
    }
}
```

## 实例工厂

```java
public class UserFactory {
    public User createUser(){
        return new User();
    }
}
```

这种必须创建类才能调用的方法设置如下

```xml
    <bean id="creatUser2" class="Factory.UserFactory"/>
    <bean id="UserDo" factory-bean="creatUser2" factory-method="createUser"/>
```

Main函数

```java
public class Main {
    public static void main(String[] args) {
        ApplicationContext as = new ClassPathXmlApplicationContext("applicationContext.xml");
        User user = (User) as.getBean("UserDo");
    }
}
```

但这种方法过于麻烦且臃肿，新建出来的bean有一个只是作为中间桥梁的作用，所以可以用FactoryBean

泛型是你要构造的类，isSingLetion是设置是否单例，true就是单例，false就是多例，默认就是单例

```java
public class UserFactoryBean implements FactoryBean<User> {
    @Override
    public User getObject() throws Exception {
        return new User();
    }

    @Override
    public Class<?> getObjectType() {
        return User.class;
    }
    @Override
    public boolean isSingleton() {
        return true;
    }
}
```

## Bean的生命周期

定义Bean的初始函数和销毁函数，在xml中设置**init-method**和**destroy-method**

```java
public class User {
    public User(){
        System.out.println("construction~");
    }
    public void save(){
        System.out.println("User...");
    }
    public void init(){
        System.out.println("init...");
    }
    public void destroy(){
        System.out.println("destroy...");
    }
}
```

````xml
    <bean id="User" class="com.yuanshen.pojo.User" name="user,user2"
          init-method="init" destroy-method="destroy">
    </bean>
````

但是一定要在Main里面手动关闭IoC容器才能触发销毁函数，因为在进程结束后虚拟机会直接关闭，就不会触发销毁函数

```java
public class Main {
    public static void main(String[] args) {
        ClassPathXmlApplicationContext as = new ClassPathXmlApplicationContext("applicationContext.xml");
        User user = (User) as.getBean("User");
        as.close();
    }
}
```

或者加入关闭钩子

```java
public class Main {
    public static void main(String[] args) {
        ClassPathXmlApplicationContext as = new ClassPathXmlApplicationContext("applicationContext.xml");
        User user = (User) as.getBean("User");
        as.registerShutdownHook();
    }
}
```

初始化和销毁也可以用接口固定格式来写

```java
public class User implements InitializingBean, DisposableBean {
    public User(){
        System.out.println("construction~");
    }
    public void save(){
        System.out.println("User...");
    }

    @Override
    public void destroy() throws Exception {
        System.out.println("destroy...");
    }

    @Override
    public void afterPropertiesSet() throws Exception {
        System.out.println("init");
    }
}
```

xml文件就不懂绑定init-method和destroy-method了

```xml
    <bean id="User" class="com.yuanshen.pojo.User" name="user,user2"></bean>
```

## 注入

### setter注入

标签是<property\>,name是类里面的形参，ref是Bean

```xml
    <bean id="Student" class="com.yuanshen.pojo.Student" name="student student2" scope="prototype">
        <property name="user1" ref="User"/>
        <property name="user2" ref="User"/>
    </bean>
```

```java
public class Student {
    private User user1;
    private User user2;

    public void save() {
        System.out.println("Student..");
        user1.save();
    }

    public void setUser1(User user1) {
        this.user1 = user1;
        System.out.println("注入1");
    }

    public void setUser2(User user2) {
        System.out.println("注入2");
        this.user1 = user2;
    }
}
```

如果是普通参数，那就把ref改成value

```xml
    <bean id="Student" class="com.yuanshen.pojo.Student" name="student student2" scope="prototype">
        <property name="age" value="18"/>
        <property name="name" value="李四"/>
    </bean>
```

<property\>的顺序也可以改变

```java
public class Student {
    String name;
    int age;

    public void save() {
        System.out.println("Student..");
    }

    public void setName(String name) {
        System.out.println(name);
        this.name = name;
    }

    public void setAge(int age) {
        System.out.println(age);
        this.age = age;
    }
}
```

### 构造器注入

标签是<constructor\>其他和setter差不多

```xml
    <bean id="Student" class="com.yuanshen.pojo.Student" name="student student2" scope="prototype">
        <constructor-arg name="name" value="张三"/>
        <constructor-arg name="age" value="18"/>
    </bean>
```

```java
public class Student {
    String name;
    int age;
    public Student(String name, int age) {
        this.name = name;
        this.age = age;
        System.out.println(name + " " + age);
    }
    public void save() {
        System.out.println("Student..");
    }
}
```

引用类型也是把value改成ref就行了

### 自动注入

在<bean\>中加入autowire="类型"就可以开启自动注入了(仅限引用),byType就是按类型去IoC容器里面找，但是如果你把一个类定义了多个Bean这里就会报错，不知道找哪一个

ByName就是按名字去找，要求形参要和Bean的id名一致

```xml
<bean id="Student" class="com.yuanshen.pojo.Student" name="student student2" scope="prototype" autowire="byType"></bean>
```

```java
public class Student {
    private User user;
    public void setUser(User user) {
        System.out.println(user);
        this.user = user;
    }
    public void save() {
        System.out.println("Student..");
    }
}
```

## 集合注入

如下图，set和list一样，就把<list\>改成<set\>就可以了

```java
public class ListTest {
    private int[] arr;
    private List<String> list;
    private Map<String,String> map;
    private Properties properties;

    public void setArr(int[] arr) {
        this.arr = arr;
        System.out.println(arr);
    }

    public void setList(List<String> list) {
        this.list = list;
        System.out.println(list);
    }

    public void setMap(Map<String, String> map) {
        this.map = map;
        System.out.println(map);
    }
    public void setProperties(Properties properties) {
        this.properties = properties;
        System.out.println(properties);
    }
}
```

```xml
    <bean id="listTest" class="com.yuanshen.pojo.ListTest">
        <property name="arr">
            <array>
                <value>100</value>
                <value>200</value>
                <value>300</value>
            </array>
        </property>
        <property name="list">
            <list>
                <value>100</value>
                <value>200</value>
                <value>300</value>
            </list>
        </property>
        <property name="map">
            <map>
                <entry key="角色1" value="小可莉~"/>
                <entry key="角色2" value="小刻晴~"/>
                <entry key="角色3" value="小纳西妲~"/>
            </map>
        </property>
        <property name="properties">
            <props>
                <prop key="角色1">小可莉~</prop>
            </props>
        </property>
    </bean>
```

