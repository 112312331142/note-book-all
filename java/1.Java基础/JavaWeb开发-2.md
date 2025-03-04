## 七、Mybatis

MyBaits是一款优秀的持久层框架，用于简化JDBC的开发

官网：https://mybatis.org/mybatis-3/zh/index.html 

### 7.1 Mybatis入门

#### 7.1.1 快速入门

Mybatis操作数据库的步骤：

1. 准备工作(创建springboot工程、数据库表user、实体类User)

2. 引入Mybatis的相关依赖，配置Mybatis(数据库连接信息)

   > 项目工程创建完成后，自动在pom.xml文件中，导入Mybatis依赖和MySQL驱动依赖

   ```xml
   <!-- 仅供参考：只粘贴了pom.xml中部分内容 -->
   <dependencies>
           <!-- mybatis起步依赖 -->
           <dependency>
               <groupId>org.mybatis.spring.boot</groupId>
               <artifactId>mybatis-spring-boot-starter</artifactId>
               <version>2.3.0</version>
           </dependency>

           <!-- mysql驱动包依赖 -->
           <dependency>
               <groupId>com.mysql</groupId>
               <artifactId>mysql-connector-j</artifactId>
               <scope>runtime</scope>
           </dependency>
           
           <!-- spring单元测试 (集成了junit) -->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-test</artifactId>
               <scope>test</scope>
           </dependency>
   </dependencies>
   ```

3. 编写SQL语句(注解/XML)

#### 7.1.2 JDBC介绍

JDBC：（Java DataBase Connectivity）,就是使用java语言操作关系型数据库的一套API

本质：

- sun公司官方定义的一套操作所有关系型数据库的规范，即接口
- 各个数据厂商去实现这套接口，提供数据库驱动jar包
- 可以使用这套（JDBC）编程，真正执行的代码是驱动jar包中的实现类

JDBC具体代码实现：

```java
import com.itheima.pojo.User;
import org.junit.jupiter.api.Test;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;
import java.util.ArrayList;
import java.util.List;

public class JdbcTest {
    @Test
    public void testJdbc() throws Exception {
        //1. 注册驱动
        Class.forName("com.mysql.cj.jdbc.Driver");

        //2. 获取数据库连接
        String url="jdbc:mysql://127.0.0.1:3306/mybatis";
        String username = "root";
        String password = "1234";
        Connection connection = DriverManager.getConnection(url, username, password);

        //3. 执行SQL
        Statement statement = connection.createStatement(); //操作SQL的对象
        String sql="select id,name,age,gender,phone from user";
        ResultSet rs = statement.executeQuery(sql);//SQL查询结果会封装在ResultSet对象中

        List<User> userList = new ArrayList<>();//集合对象（用于存储User对象）
        //4. 处理SQL执行结果
        while (rs.next()){
            //取出一行记录中id、name、age、gender、phone下的数据
            int id = rs.getInt("id");
            String name = rs.getString("name");
            short age = rs.getShort("age");
            short gender = rs.getShort("gender");
            String phone = rs.getString("phone");
            //把一行记录中的数据，封装到User对象中
            User user = new User(id,name,age,gender,phone);
            userList.add(user);//User对象添加到集合
        }
        //5. 释放资源
        statement.close();
        connection.close();
        rs.close();

        //遍历集合
        for (User user : userList) {
            System.out.println(user);
        }
    }
}
```

> DriverManager(类)：数据库驱动管理类。
>
> - 作用：
>   1. 注册驱动
>   2. 创建java代码和数据库之间的连接，即获取Connection对象
>
> Connection(接口)：建立数据库连接的对象
>
> - 作用：用于建立java程序和数据库之间的连接
>
> Statement(接口)： 数据库操作对象(执行SQL语句的对象)。
>
> - 作用：用于向数据库发送sql语句
>
> ResultSet(接口)：结果集对象（一张虚拟表）
>
> - 作用：sql查询语句的执行结果会封装在ResultSet中

通过上述代码，我们看到直接基于JDBC程序来操作数据库，代码实现非常繁琐，所以在项目开发中，我们很少使用。  在项目开发中，通常会使用Mybatis这类的高级技术来操作数据库，从而简化数据库操作、提高开发效率。

#### 7.1.3 数据库连接池

数据库连接池是个容器，负责分配、管理数据库连接(Connection)

- 程序在启动时，会在数据库连接池(容器)中，创建一定数量的Connection对象

允许应用程序重复使用一个现有的数据库连接，而不是再重新建立一个

- 客户端在执行SQL时，先从连接池中获取一个Connection对象，然后在执行SQL语句，SQL语句执行完之后，释放Connection时就会把Connection对象归还给连接池（Connection对象可以复用）

释放空闲时间超过最大空闲时间的连接，来避免因为没有释放连接而引起的数据库连接遗漏

- 客户端获取到Connection对象了，但是Connection对象并没有去访问数据库(处于空闲)，数据库连接池发现Connection对象的空闲时间 > 连接池中预设的最大空闲时间，此时数据库连接池就会自动释放掉这个连接对象

优势：

- 资源重用
- 提升系统响应速度
- 避免数据库连接遗漏

标准接口：DateSource

- 官方（sun）提供的数据库连接池接口，由第三方组织实现此接口
- 功能：获取连接：`public Connection getConnection() throws SQLException;`

常见产品：

- C3P0：

- DBCP：

- Druid（德鲁伊）：是阿里巴巴开源的数据连接池项目

  参考官方地址：https://github.com/alibaba/druid/tree/master/druid-spring-boot-starter

- Hikari：springboot默认

#### 7.1.4 lombok工具包

lombok是一个使用的java类库，能通过注解的形式自动生成构造器、getter/setter、equals、hashcode、toString等方法，并可以自动化生成日志变量，简化Java开发、提高效率

| **注解**            | **作用**                                                     |
| ------------------- | ------------------------------------------------------------ |
| @Getter/@Setter     | 为所有的属性提供get/set方法                                  |
| @ToString           | 会给类自动生成易阅读的  toString 方法                        |
| @EqualsAndHashCode  | 根据类所拥有的非静态字段自动重写 equals 方法和  hashCode 方法 |
| @Data               | 提供了更综合的生成代码功能（@Getter  + @Setter + @ToString + @EqualsAndHashCode） |
| @NoArgsConstructor  | 为实体类生成无参的构造器方法                                 |
| @AllArgsConstructor | 为实体类生成除了static修饰的字段之外带有各参数的构造器方法。 |

Lombok会在编译时，自动生成对应的java代码，使用lombok时，还需要安装lombok的插件（idea自带）

### 7.2 Mybatis基础增删改查

#### 7.2.1 环境准备

实施前的准备工作：

1. 准备数据库表
2. 创建一个新的springboot工程，选择引入对应的起步依赖（mybatis、mysql驱动、lombok）
3. application.properties中引入数据库连接信息
4. 创建对应的实体类 Emp（实体类属性采用驼峰命名）
5. 准备Mapper接口 EmpMapper

#### 7.2.2Mybatis基础操作-删除

##### 7.2.2.1 功能实现

SQL语句：

```mysql
delete from emp where id=17;
```

接口方法：

```java
@Delete("delete from emp where id=#{id}")
public void delete(Integer id);
```

注：如果mapper接口方法形参只有一个普通类型的参数，#{...}里面的属性名可以随便写，如：#{id}、#{value}

##### 7.2.2.2 日志输入

在Mybatis当中我们可以借助日志，查看到sql语句的执行、执行传递的参数以及执行结果。具体操作如下：

1. 打开application.properties文件
2. 开启mybatis的日志，并指定输出到控制台

```properties
#指定mybatis输出日志的位置, 输出控制台
mybatis.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
```

开启日志之后，我们再次运行单元测试，可以看到在控制台中，输出了以下的SQL语句信息：

![image-20220901164225644](img\%E6%97%A5%E5%BF%97%E8%BE%93%E5%85%A5.png) 

> 但是我们发现输出的SQL语句：delete from emp where id = ?，我们输入的参数16并没有在后面拼接，id的值是使用?进行占位。那这种SQL语句我们称为预编译SQL。

##### 7.2.2.3 预编译SQL

预编译SQL优势：

- 性能更高
- 更安全（防止SQL注入）

![img](img/%E9%A2%84%E7%BC%96%E8%AF%91SQL%E6%B5%81%E7%A8%8B.png)

SQL注入是通过操作输入的数据来修改先定义好的SQL语句，以达到执行代码对服务器进行攻击的方法

##### 7.2.2.4 参数占位符

- `#{...}`

  执行SQL时，会将`#{...}`替换为？，生成预编译SQL，会自动设置参数值

  使用时机：参数传递，都使用#{...}

- `${...}`

  拼接SQL，直接将参数拼接到SQL语句中，存在SQL注入问题

  使用时机：如果对表名、列表进行动态设置时使用

#### 7.2.3 Mybatis基础操作-新增

##### 7.2.3.1 功能实现

SQL语句：

```mysql
insert into emp (username, name, gender, image, job, entrydate, dept_id, create_time, update_time)
values ('gaoxin','高鑫',1,'1.jpg','2022-09-01',1,now(),now());
```

接口方法：

```java
//新增员工操作
@Insert("insert into emp (username, name, gender, image, job, entrydate," +
        " dept_id, create_time, update_time)\n" +
        "values (#{username},#{name},#{gender},#{image},#{job},#{entrydate},#{deptId}," +
        "#{createTime},#{updateTime});")
public void insert(Emp emp);
```

##### 7.2.3.2 主键返回

描述：在数据添加成功后，需要获取插入数据库数据的主键

默认情况下，执行插入操作时，是不会主键值返回的。如果我们想要拿到主键值，需要在Mapper接口中的方法上添加一个Options注解，并在注解中指定属性useGeneratedKeys=true和keyProperty="实体类属性名

实现方法：

```java
//新增员工操作
 @Options(keyProperty = "id", useGeneratedKeys = true)
 @Insert("insert into emp (username, name, gender, image, job, entrydate," +
         " dept_id, create_time, update_time)\n" +
         "values (#{username},#{name},#{gender},#{image},#{job},#{entrydate}" +
         ",#{deptId},#{createTime},#{updateTime});")
 public void insert(Emp emp);
```

#### 7.2.3 Mybatis基础操作-更新

SQL语句：

```mysql
update emp set username = '',name = '',gender = '',image = '',job = '',entrydate = '',dept_id = '',update_time = '' where id = 1;
```

接口方法：

```java
//更新员工
@Update("update emp set username = #{username},name = #{name},gender = #{gender}," +
        "image = #{image},job = #{job},entrydate = #{entrydate},dept_id = #{deptId}," +
        "update_time = #{updateTime} where id = #{id}")
public void update(Emp emp);
```

#### 7.2.3 Mybatis基础操作-查询

##### 7.2.3.1 根据ID查询

SQL语句：

```mysql
-- 根据ID查询员工
select * from emp where id = 20;
```

接口方法：

```java
//查询员工id
@Select("select * from emp where id = #{id}")
public Emp getEmpById(Integer id);
```

##### 7.2.3.2 数据封装

我们看到查询返回的结果中大部分字段是有值的，但是deptId，createTime，updateTime这几个字段是没有值的，而数据库中是有对应的字段值的，这是为什么呢？

原因：实体类属性名和数据库表查询返回的字段名一致，mybatis会自动封装；不一致，不能自动封装

解决方案：

1. 解决方案1：给字段起别名，让别名与实体类属性一致

   ```java
    //解决方案1：给字段起别名，让别名与实体类属性一致
    @Select("select id, username, password, name, gender, image, job, entrydate," +
            " dept_id deptId, create_time createTime, update_time updateTime" +
            " from emp where id = #{id}")
    public Emp getEmpById(Integer id);
   ```


1. 解决方案2：通过@Results，@Result注解手动映射封装

   ```java
    //解决方案2：通过@Results，@Result注解手动映射封装
    @Results({
            @Result(column = "dept_id",property = "deptId"),
            @Result(column = "create_time",property = "createTime"),
            @Result(column = "update_time",property = "updateTime")
    })
    @Select("select * from emp where id = #{id}")
    public Emp getEmpById(Integer id);
   ```

2. 开启mybatis的驼峰命名自动映射开关（推荐）

   ```properties
   # 开启mybatis的驼峰命名自动映射开关,即从数据库字段名a_column映射到java属性名aColumn
   mybatis.configuration.map-underscore-to-camel-case=true
   ```

##### 7.2.3.3 条件查询

SQL语句：

```mysql
select *from emp where name like '%张%' and gender = 1 and
                     entrydate between '2010-01-01' and '2020-01-10' order by  update_time desc;
```

接口方法：

```java
//条件查询员工
@Select("select *from emp where name like '%${name}%' and gender = #{gender} and " +
        "entrydate between #{begin} and #{end} order by  update_time desc;")
public List<Emp> list(String name, Short gender, LocalDate begin, LocalDate end);
```

使用MySQL提供的字符串拼接函数：concat('%' , '关键字' , '%')解决SQL注入问题

```java
//条件查询员工
@Select("select *from emp where name like concat('%',#{name},'%') and gender = #{gender} and " +
            "entrydate between #{begin} and #{end} order by  update_time desc;")
public List<Emp> list(String name, short gender, LocalDate begin, LocalDate end);
```

### 7.3 XML映射文件

Mybatis的开发有两种方式：注解、XML

书写规范：

- XML映射文件的名称与Mapper接口名称一致，并且将XML映射文件和Mapper接口放置在相同包下（同包同名）
- XML映射文件的namespace属性为Mapper接口全限定名一致
- XML映射文件中sql语句的id与Mapper接口中的方法名一致，并保持返回类型一致

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.itheima.mapper.EmpMapper">
<!--    resultType:单条记录所封装的类型-->
    <select id="list" resultType="com.itheima.pojo.Emp">
        select *from emp where name like concat('%',#{name},'%') and gender = #{gender} and
           entrydate between #{begin} and #{end} order by  update_time desc;
    </select>
</mapper>
```

MybatisX是一款基于IDEA的快速开发Mybatis的插件，为效率而生

使用Mybatis的注解，主要是来完成一些简单的增删改查功能。如果需要实现复杂的SQL功能，建议使用XML来配置映射语句

### 7.4 Mybaits动态SQL

随着用户的输入或外部条件的变化而变化的SQL语句，我们称之为动态SQL

在Mybatis中提供了很多实现动态SQL的标签，我们学习Mybatis中的动态SQL就是掌握这些动态SQL标签。

#### 7.4.1 < if >

`<if>`：用于判断条件是否成立，使用test属性进行条件判断，如果条件为true，则拼接SQL

`<where>`：where元素只会在子元素有内容的情况下才插入where子句，而且会自动去除子句中开头的and或or

`<set>`：动态的在SQL语句行首中插入set关键字，并会删掉额外的逗号。（用于update语句中）

```xml
<select id="list" resultType="com.itheima.pojo.Emp">
      select *
      from emp
      <where>
      <if test="name !=null">
          name like concat('%', #{name}, '%')
      </if>
      <if test="gender !=null">
          and gender = #{gender}
      </if>
      <if test="begin !=null and end !=null">
          and entrydate between #{begin} and #{end}
      </if>
      </where>
      order by update_time desc;
  </select>
```

#### 7.4.2 < foreach >

使用`<foreach>`遍历deleteByIds方法中传递的参数ids集合

```xml
<!--批量删除员工操作-->
    <!--
        collection:遍历的集合
        item:遍历的元素
        separator:分隔符
        open:遍历开始前拼接的SQL片段
        close:遍历结束后拼接的SQL片段
    -->
<delete id="deleteById">
    delete
    from emp
    where id in
    <foreach collection="ids" item="id" separator="," open="(" close=")">
        #{id}
    </foreach>
</delete>
```

#### 7.4.3 < sql > 和 < include >

我们可以对重复的代码片段进行抽取，将其通过`<sql>`标签封装到一个SQL片段，然后再通过`<include>`标签进行引用。

- `<sql>`：定义可重用的SQL片段
- `<include>`：通过属性refid，指定包含的SQL片段

```xml
<sql id="commonSelect">
 	select id, username, password, name, gender, image, job, entrydate, dept_id, create_time, update_time from emp
</sql>
```

```xml
<select id="list" resultType="com.itheima.pojo.Emp">
    <include refid="commonSelect"/>
    <where>
        <if test="name != null">
            name like concat('%',#{name},'%')
        </if>
        <if test="gender != null">
            and gender = #{gender}
        </if>
        <if test="begin != null and end != null">
            and entrydate between #{begin} and #{end}
        </if>
    </where>
    order by update_time desc
</select>
```



## 八、案例

[案例地址](D:\Java\javawenjian\example)

[笔记地址](C:\downFile\笔记本\java\javaSpringBoot案例\SpringBootStudy)

### 8.1 文件上传

#### 8.1.1 文件上传介绍

文件上传指将本地图片、视频、音频等文件上传到服务器，供其他用户浏览或下载的过程

想要完成文件上传这个功能需要涉及到两个部分：

1. 前端程序
2. 服务端程序

我们先来看看在前端程序中要完成哪些代码：

```html
<form action="/upload" method="post" enctype="multipart/form-data">
	姓名: <input type="text" name="username"><br>
    年龄: <input type="text" name="age"><br>
    头像: <input type="file" name="image"><br>
    <input type="submit" value="提交">
</form>
```

上传文件的原始form表单，要求表单必须具备以下三点（上传文件页面三要素）：

- 表单必须有file域，用于选择要上传的文件

  > ```html
  > <input type="file" name="image"/>
  > ```

- 表单提交方式必须为POST

  > 通常上传的文件会比较大，所以需要使用 POST 提交方式

- 表单的编码类型enctype必须要设置为：multipart/form-data

  > 普通默认的编码格式是不适合传输大型的二进制数据的，所以在文件上传时，表单的编码格式必须设置为multipart/form-data

知道了前端程序中需要设置上传文件页面三要素，那我们的后端程序又是如何实现的：

- 首先在服务端定义这么一个controller，用来进行文件上传，然后在controller当中定义一个方法来处理`/upload` 请求

- 在定义的方法中接收提交过来的数据 （方法中的形参名和请求参数的名字保持一致）

  - 用户名：String  name
  - 年龄： Integer  age
  - 文件： MultipartFile  image

  > Spring中提供了一个API：MultipartFile，使用这个API就可以来接收到上传的文件

```java
package com.chen.controller;

import com.chen.pojo.Result;
import lombok.extern.slf4j.Slf4j;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.multipart.MultipartFile;

@Slf4j
@RestController
public class UploadController {

    @PostMapping("upload")
    public Result upload(String username, Integer age, MultipartFile image) {
        log.info("文件上传：{}，{}，{}", username, age, image);
        return Result.success();
    }

}

```

#### 8.1.2 本地存储

在服务端，接收到上传来的文件之后，将文件存储在本地服务器磁盘中

MultipartFile 常见方法： 

- String  getOriginalFilename();  //获取原始文件名
- void  transferTo(File dest);     //将接收的文件转存到磁盘文件中
- long  getSize();     //获取文件的大小，单位：字节
- byte[]  getBytes();    //获取文件内容的字节数组
- InputStream  getInputStream();    //获取接收到的文件内容的输入流

```java
@Slf4j
@RestController
public class UploadController {

    @PostMapping("upload")
    public Result upload(String username, Integer age, MultipartFile image) throws Exception {
        log.info("文件上传：{}，{}，{}", username, age, image);

        //获取原始文件名
        String originalFilename = image.getOriginalFilename();

        //构造唯一的文件名（不能重复） --uuid（通用唯一识别码）
        assert originalFilename != null;
        int index = originalFilename.lastIndexOf(".");
        String suffix = originalFilename.substring(index);
        String newFileName = UUID.randomUUID().toString() + suffix;

        log.info("新的文件名：{}", newFileName);

        //将文件存在在服务器的磁盘目录中 D:\Java\javawenjian\tlias-web-management\image
        image.transferTo(new File("D:\\Java\\javawenjian\\tlias-web-management\\image\\"
                +newFileName));

        return Result.success();
    }

}

```

在SpringBoot中，文件上传时默认单个文件最大大小为1M

那么如果需要上传大文件，可以在application.properties进行如下配置：

```properties
#配置单个文件最大上传大小
spring.servlet.multipart.max-file-size=10MB

#配置单个请求最大上传大小(一次请求可以上传多个文件)
spring.servlet.multipart.max-request-size=100MB
```

#### 8.1.3 阿里云OSS

阿里云是阿里巴巴集团旗下全球领先的云计算公司，也是中国最大的云服务提供商

阿里云对象存储服务（Object Storage Service，简称OSS）为您提供基于网络的数据存取服务。使用OSS，您可以通过网络随时存储和调用包括文本、图片、音频和视频等在内的各种非结构化数据文件。

阿里云OSS将数据文件以对象（object）的形式上传到存储空间（bucket）中。

SDK：Software Developement Kit缩写，软件工具包，包括辅助软件开发的依赖（jar包），代码示例等，都可以叫做SDK

 Bucket：存储空间是用户存储对象（Object，就是文件）的容器，所有对象都必须隶属于某个存储空间

[笔记](C:\downFile\笔记本\java\阿里云OSS\AliOSS.md)

### 8.2 配置文件

#### 8.2.1 参数配置化

在我们之前编写的程序中进行文件上传时，需要调用AliOSSUtils工具类，将文件上传到阿里云OSS对象存储服务当中。而在调用工具类进行文件上传时，需要一些参数：

- endpoint       //阿里云OSS域名
- accessKeyID    //用户身份ID
- accessKeySecret   //用户密钥
- bucketName      //存储空间的名字

关于以上的这些阿里云相关配置信息，我们是直接写死在java代码中了(硬编码)，如果我们在做项目时每涉及到一个第三方技术服务，就将其参数硬编码，那么在Java程序中会存在两个问题：

1. 如果这些参数发生变化了，就必须在源程序代码中改动这些参数，然后需要重新进行代码的编译，将Java代码编译成class字节码文件再重新运行程序。（比较繁琐）
2. 如果我们开发的是一个真实的企业级项目， Java类可能会有很多，如果将这些参数分散的定义在各个Java类当中，我们要修改一个参数值，我们就需要在众多的Java代码当中来定位到对应的位置，再来修改参数，修改完毕之后再重新编译再运行。（参数配置过于分散，是不方便集中的管理和维护）

为了解决以上分析的问题，我们可以将参数配置在配置文件中。如下：

```properties
#自定义的阿里云OSS配置信息
aliyun.oss.endpoint=https://oss-cn-hangzhou.aliyuncs.com
aliyun.oss.accessKeyId=LTAI4GCH1vX6DKqJWxd6nEuW
aliyun.oss.accessKeySecret=yBshYweHOpqDuhCArrVHwIiBKpyqSL
aliyun.oss.bucketName=web-tlias
```

在将阿里云OSS配置参数交给properties配置文件来管理之后，我们的AliOSSUtils工具类就变为以下形式：

```java
@Component
public class AliOSSUtils {
    /*以下4个参数没有指定值（默认值：null）*/
    private String endpoint;
    private String accessKeyId;
    private String accessKeySecret;
    private String bucketName;

    //省略其他代码...
}
```

> 而此时如果直接调用AliOSSUtils类当中的upload方法进行文件上传时，这4项参数全部为null，原因是因为并没有给它赋值。
>
> 此时我们是不是需要将配置文件当中所配置的属性值读取出来，并分别赋值给AliOSSUtils工具类当中的各个属性呢？那应该怎么做呢？

因为application.properties是springboot项目默认的配置文件，所以springboot程序在启动时会默认读取application.properties配置文件，而我们可以使用一个现成的注解：@Value，获取配置文件中的数据。

@Value 注解通常用于外部配置的属性注入，具体用法为： @Value("${配置文件中的key}")

```java
@Component
public class AliOSSUtils {

    @Value("${aliyun.oss.endpoint}")
    private String endpoint;
    
    @Value("${aliyun.oss.accessKeyId}")
    private String accessKeyId;
    
    @Value("${aliyun.oss.accessKeySecret}")
    private String accessKeySecret;
    
    @Value("${aliyun.oss.bucketName}")
    private String bucketName;
 	
 	//省略其他代码...
 }   
```

#### 8.2.2 yml配置文件

SpringBoot提供了多种属性配置方式

- application.properties

  ```properties
  server.port=8080
  server.address=127.0.0.1
  ```

- application.yml 

  ```yml
  server:
    port: 8080
    address: 127.0.0.1
  ```

- application.yaml 

  ```yaml
  server:
    port: 8080
    address: 127.0.0.1
  ```

> yml 格式的配置文件，后缀名有两种：
>
> - yml （推荐）
> - yaml

常见配置文件格式对比：

![件配置格式](img\%E6%96%87%E4%BB%B6%E9%85%8D%E7%BD%AE%E6%A0%BC%E5%BC%8F%E6%AF%94.png)

yml基本语法：

- 大小写敏感
- 数值前边必须有空格，作为分隔符
- 使用缩进表示层级关系，缩进时，不允许使用Tab键，只能用空格（idea中会自动将Tab转换为空格）
- 缩进的空格数目不重要，只要相同层级的元素左侧对齐即可
- `#`表示注释，从这个字符一直到行尾，都会被解析器忽略

yml文件中常见的数据格式：

1. 定义对象或Map集合
2. 定义数组、list或set集合

对象/Map集合

```yml
user:
  name: zhangsan
  age: 18
  password: 123456
```

数组/List/Set集合

```yml
hobby: 
  - java
  - game
  - sport
```

熟悉完了yml文件的基本语法后，我们修改下之前案例中使用的配置文件，变更为application.yml配置方式：

1. 修改application.properties名字为：`_application.properties`（名字随便更换，只要加载不到即可）
2. 创建新的配置文件： `application.yml`

#### 8.2.3 @ConfigurationProperties

需要注入的属性较多会比较繁琐。在Spring中给我们提供了一种简化方式，可以直接将配置文件中配置项的值自动的注入到对象的属性中。

Spring提供的简化方式套路：

1. 需要创建一个实现类，且实体类中的属性名和配置文件当中key的名字必须要一致

   > 比如：配置文件当中叫endpoints，实体类当中的属性也得叫endpoints，另外实体类当中的属性还需要提供 getter / setter方法

2. 需要将实体类交给Spring的IOC容器管理，成为IOC容器当中的bean对象

3. 在实体类上添加`@ConfigurationProperties`注解，并通过perfect属性来指定配置参数项的前缀

@ConfigurationProperties和@Value注解：

相同点：都是用来注入外部配置的属性的。

不同点：

- @Value注解只能一个一个的进行外部属性的注入。
- @ConfigurationProperties可以批量的将外部的属性配置注入到bean对象的属性中。

如果要注入的属性非常的多，并且还想做到复用，就可以定义这么一个bean对象。通过 configuration properties 批量的将外部的属性配置直接注入到 bin 对象的属性当中。在其他的类当中，我要想获取到注入进来的属性，我直接注入 bin 对象，然后调用 get 方法，就可以获取到对应的属性值了

## 九、 登录校验

### 9.1 会话技术

会话：用户打开浏览器，访问web服务器的资源，会话建立，直到有一方断开连接，会话结束。在一次会话中可以包含多次请求和响应

会话跟踪：一种维护浏览器状态的方法，服务器需要识别多次请求是否来自于同一浏览器，以便在同一会话的多次请求间共享数据

会话跟踪方案：

- 客户端会话跟踪技术：Cookie
- 服务端会话跟踪技术：Session
- 令牌技术

#### 9.1.1 Cookie技术

cookie 是客户端会话跟踪技术，它是存储在客户端浏览器的，我们使用 cookie 来跟踪会话，我们就可以在浏览器第一次发起请求来请求服务器的时候，我们在服务器端来设置一个cookie。

比如第一次请求了登录接口，登录接口执行完成之后，我们就可以设置一个cookie，在 cookie 当中我们就可以来存储用户相关的一些数据信息。比如我可以在 cookie 当中来存储当前登录用户的用户名，用户的ID。

服务器端在给客户端在响应数据的时候，会**自动**的将 cookie 响应给浏览器，浏览器接收到响应回来的 cookie 之后，会**自动**的将 cookie 的值存储在浏览器本地。接下来在后续的每一次请求当中，都会将浏览器本地所存储的 cookie **自动**地携带到服务端。 

接下来在服务端我们就可以获取到 cookie 的值。我们可以去判断一下这个 cookie 的值是否存在，如果不存在这个cookie，就说明客户端之前是没有访问登录接口的；如果存在 cookie 的值，就说明客户端之前已经登录完成了。这样我们就可以基于 cookie 在同一次会话的不同请求之间来共享数据。

3 个自动：

- 服务器会 **自动** 的将 cookie 响应给浏览器。
- 浏览器接收到响应回来的数据之后，会 **自动** 的将 cookie 存储在浏览器本地。
- 在后续的请求当中，浏览器会 **自动** 的将 cookie 携带到服务器端。

**为什么这一切都是自动化进行的？**

是因为 cookie 它是 HTP 协议当中所支持的技术，而各大浏览器厂商都支持了这一标准。在 HTTP 协议官方给我们提供了一个响应头和请求头：

- 响应头 Set-Cookie ：设置Cookie数据的
- 请求头 Cookie：携带Cookie数据的

**优缺点**

- 优点：HTTP协议中支持的技术（像Set-Cookie 响应头的解析以及 Cookie 请求头数据的携带，都是浏览器自动进行的，是无需我们手动操作的）
- 缺点：
  - 移动端APP(Android、IOS)中无法使用Cookie
  - 不安全，用户可以自己禁用Cookie
  - Cookie不能跨域

区分跨域的维度：协议、IP/协议、端口，只要上述的三个维度有任何一个维度不同，那就是跨域操作

#### 9.1.2 Session技术

- 获取Session 

  如果我们现在要基于 Session 来进行会话跟踪，浏览器在第一次请求服务器的时候，我们就可以直接在服务器当中来获取到会话对象Session。如果是第一次请求Session ，会话对象是不存在的，这个时候服务器会自动的创建一个会话对象Session 。而每一个会话对象Session ，它都有一个ID（示意图中Session后面括号中的1，就表示ID），我们称之为 Session 的ID。


- 响应Cookie (JSESSIONID) 

  接下来，服务器端在给浏览器响应数据的时候，它会将 Session 的 ID 通过 Cookie 响应给浏览器。其实在响应头当中增加了一个 Set-Cookie 响应头。这个  Set-Cookie  响应头对应的值是不是cookie？ cookie 的名字是固定的 JSESSIONID 代表的服务器端会话对象 Session 的 ID。浏览器会自动识别这个响应头，然后自动将Cookie存储在浏览器本地。


- 查找Session 

  接下来，在后续的每一次请求当中，都会将 Cookie 的数据获取出来，并且携带到服务端。接下来服务器拿到JSESSIONID这个 Cookie 的值，也就是 Session 的ID。拿到 ID 之后，就会从众多的 Session 当中来找到当前请求对应的会话对象Session。

**优缺点**

- 优点：Session是存储在服务端的，安全
- 缺点：
  - 服务器集群环境下无法直接使用Session
  - Cookie的所有缺点

> Session 底层是基于Cookie实现的会话跟踪，如果Cookie不可用，则该方案，也就失效了。

#### 9.1.3  令牌技术

优点：

* 支持PC端，移动端
* 解决集群环境下的认证问题
* 减轻服务器端存储压力

缺点：需要自己实现

### 9.2  JWT令牌

#### 9.2.1  JWT令牌介绍

JSON web Token（JWT），（官网：https://jwt.io/）

定义了一种简洁、自包含的格式，用于在通信双方都以json数据格式安全的传输信息。由于数字签名的存在，这些信息都是可靠的

组成：（JWT令牌由三个部分组成，三个部分之间使用英文的点来分割）

* Head（头）：记录令牌类型，签名算法，例如（"alg":"HS256","type":"JWT")
* Payload（有效载荷）：携带一些自定义信息、默认信息等。例如（"id":1,"username"："Tom"）
* Signature（签名）：防止Token被纂改、确保安全性。将header、payload，并加入指定密钥，通过指定签名算法计算而来

JWT是如何将原始的JSON格式数据，转变为字符串的：

> 其实在生成JWT令牌时，会对JSON格式的数据进行一次编码：进行base64编码
>
> Base64：是一种基于64个可打印的字符来表示二进制数据的编码方式。既然能编码，那也就意味着也能解码。所使用的64个字符分别是A到Z、a到z、 0- 9，一个加号，一个斜杠，加起来就是64个字符。任何数据经过base64编码之后，最终就会通过这64个字符来表示。当然还有一个符号，那就是等号。等号它是一个补位的符号
>
> 需要注意的是Base64是编码方式，而不是加密方式。

#### 9.2.2  JWT令牌生成和校验

生成JWT令牌：

```java
/**
 * 生成JWT
 */
@Test
public void testGenJWT(){
    Map<String, Object> claims=new HashMap<>();
    claims.put("id","22");
    claims.put("name","tom");

    String jwt = Jwts.builder()
            .signWith(SignatureAlgorithm.HS256, "chen")//签名算法
            .setClaims(claims)//自定义内容（载荷）
            .setExpiration(new Date(System.currentTimeMillis()))//设置有效期qh
            .compact();

    System.out.println(jwt);
}
```

解析JWT令牌：

```java
/**
 * 解析JWT
 */
@Test
public void testParseJwt(){
    Claims claims = Jwts.parser()
            .setSigningKey("chen")
            .parseClaimsJws("eyJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoidG9 tIiwiaWQiOiIyMiIsImV4cC I6MTcy NjAzNDY0OX0.LRVCK82eUXppKDMUaOZvoncziKFSlUapp69O4jU7dyg")
            .getBody();
    System.out.println(claims);
}
```

注：

* Jwt校验时使用的签名密钥，必须和生成Jwt令牌时使用的密钥是配套的
* 如果JWT令牌解析校验时保存，则说明令牌被纂改或失效了，临牌非法

#### 9.2.3 JWT令牌登陆后下发令牌

通过JWT令牌技术来跟踪会话，主要就是两步操作：

1. 生成令牌
   - 在登录成功之后来生成一个JWT令牌，并且把这个令牌直接返回给前端
2. 校验令牌
   - 拦截前端请求，从请求中获取到令牌，对令牌进行解析校验

### 9.3 过滤器Filiter

概念：Filiter过滤器，是JavaWeb三大组件（Servlet、Filter、Listener）之一

过滤器可以把对资源的请求拦截下来，从而实现一些特殊的功能

过滤器一般完成一些通用的操作，比如：登录校验、统一编码处理、敏感字符处理等

#### 9.3.1 快速入门

1. 定义Filter：定义一个类，实现Filter接口，并重写其所有方法
2. 配置Filter：Filter类加@WebFilter注解，配置拦截资源的路径，引导类上加QServletComponentScan开启Servlet组件支持

```java
//@WebFilter，并指定属性urlPatterns，通过这个属性指定过滤器要拦截哪些请求
@WebFilter(urlPatterns = "/*")
public class DemoFilter implements Filter {

    @Override//初始化方法只调用一次
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("init初始化方法执行了");
        Filter.super.init(filterConfig);
    }

    @Override//拦截到请求之后调用，调用多次
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("拦截到了请求");

        //放行
        filterChain.doFilter(servletRequest, servletResponse);
    }

    @Override//销毁方法，调用多次
    public void destroy() {
        System.out.println("destroy销毁方法执行了");
        Filter.super.destroy();
    }
}
```

#### 9.3.2 执行流程

过滤器当中我们拦截到了请求之后，如果希望继续访问后面的web资源，就要执行放行操作，放行就是调用 FilterChain对象当中的doFilter()方法，在调用doFilter()这个方法之前所编写的代码属于放行之前的逻辑。

在放行后访问完 web 资源之后还会回到过滤器当中，回到过滤器之后如有需求还可以执行放行之后的逻辑，放行之后的逻辑我们写在doFilter()这行代码之后。

#### 9.3.3 拦截路径

Filter可以根据需求，配置不同的拦截资源路径：

| 拦截路径     | urlPatterns值 | 含义                              |
| ------------ | ------------- | --------------------------------- |
| 拦截具体路径 | /login        | 只有访问/login路径时，才会被拦截  |
| 目录拦截     | /emps/*       | 访问/emps下的所有资源，都会被拦截 |
| 拦截所有     | /*            | 访问所有资源，都会被拦截          |

#### 9.3.4 过滤器链

* 介绍：在一个web程序中，可以配置多个过滤器，这多个过滤器就形成了一个过滤器链
* 顺序：注解配置的Filter，优先级是按照过滤器类名（字符串）的自然排序

而这个链上的过滤器在执行的时候会一个一个的执行，会先执行第一个Filter，放行之后再来执行第二个Filter，如果执行到了最后一个过滤器放行之后，才会访问对应的web资源。

访问完web资源之后，按照我们刚才所介绍的过滤器的执行流程，还会回到过滤器当中来执行过滤器放行后的逻辑，而在执行放行后的逻辑的时候，顺序是反着的。

先要执行过滤器2放行之后的逻辑，再来执行过滤器1放行之后的逻辑，最后在给浏览器响应数据。

#### 9.3.5 登录校验

代码示例：

```java
@Slf4j
@WebFilter(urlPatterns = "/*")
public class LoginCheckFilter implements Filter {

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse,
                         FilterChain filterChain) throws IOException, ServletException {
        HttpServletRequest request = (HttpServletRequest) servletRequest;
        HttpServletResponse response = (HttpServletResponse) servletResponse;
        //1.获取请求url
        String uri = request.getRequestURI();
        log.info("请求的url:{}", uri);
        //2.判断请求url是否包含login，如果包含，说明是登陆操作，放行
        if(uri.contains("/login")){
            log.info("放行");
            filterChain.doFilter(request, response);
            return;
        }
        //3.获取请求头中的令牌(token)
        String jwt = request.getHeader("token");
        //4.判断令牌是否存在，如果不存在，返回错误结果（未登录）
        if(!StringUtils.hasText(jwt)){
            log.info("请求头token为空，返回未登陆的信息");
            Result error = Result.error("NOT_LOGIN");
            //手动转换，将对象转换为json格式的对象------》alibabaJSON tool jar
            String noLogin = JSONObject.toJSONString(error);
            response.getWriter().write(noLogin);
            return;
        }
        //5.解析token，如果解析失败，返回错误结果
        try {
            JwtUtils.parseJWT(jwt);
        } catch (Exception e) {//jwt解析失败
            log.error("error:{}", e.getMessage());
            log.info("解析令牌失败，但未登陆的错误信息");
            Result error = Result.error("NOT_LOGIN");
            //手动转换，将对象转换为json格式的对象------》alibabaJSON tool jar
            String noLogin = JSONObject.toJSONString(error);
            response.getWriter().write(noLogin);
            return;
        }

        //放行
        log.info("令牌合法，放行");
        filterChain.doFilter(request, response);
    }
}

```

### 9.4 拦截器Interceptor

* 概念：是一种动态拦截方法调用的机制，类似于过滤器。Spring框架中提供的，用于动态拦截控制器方法的执行
* 作用：拦截请求，在指定的方法调用前后，根据业务需要执行预先设定的代码

#### 9.4.1 快速入门

1. 定义拦截器：实现HandlerInterceptor接口，并重写其所有方法

   ```java
   @Component
   public class LoginCheckInterceptor implements HandlerInterceptor {

       @Override//在目标资源方法运行前执行，返回true，放行，返回false，不放行
       public boolean preHandle(HttpServletRequest request, HttpServletResponse response,
                                Object handler) throws Exception {
           System.out.println("preHandle...");
           return true;
       }

       @Override//目标资源方法运行后执行
       public void postHandle(HttpServletRequest request, HttpServletResponse response,
                              Object handler, ModelAndView modelAndView) throws Exception {
           System.out.println("postHandle...");

       }

       @Override//视图渲染完毕后运行，最后运行
       public void afterCompletion(HttpServletRequest request, HttpServletResponse response,
                                   Object handler, Exception ex) throws Exception {
           System.out.println("afterCompletion...");
       }
   }

   //request：请求对象
   //response：响应对象
   //handler：被调用的处理器对象，本质上是一个方法对象，对反射技术中的Method对象进行封装
   //modelAndView：如果处理器执行完成具有返回结果，可以读取到对应数据与页面信息，并进行调整
   //ex：如果处理器执行过程中出现异常方法，可以针对异常情况进行单独处理 
   ```

2. 注册拦截器

   ```java
   @Configuration
   public class WebConfig implements WebMvcConfigurer {

       @Autowired
       private LoginCheckInterceptor loginCheckInterceptor;

       @Override
       public void addInterceptors(InterceptorRegistry registry) {
           registry.addInterceptor(loginCheckInterceptor).addPathPatterns("/**");
       }
   }

   ```

#### 9.4.2 拦截路径

拦截路器以根据需求，配置不同的拦截路径

addPathPatterns("/**")：需要拦截的资源

excludePatterns("/login")：不需要拦截的资源

| 拦截路径  | 含义                 | 举例                                                |
| --------- | -------------------- | --------------------------------------------------- |
| /*        | 一级路径             | 能匹配/depts，/emps，/login，不能匹配 /depts/1      |
| /**       | 任意级路径           | 能匹配/depts，/depts/1，/depts/1/2                  |
| /depts/*  | /depts下的一级路径   | 能匹配/depts/1，不能匹配/depts/1/2，/depts          |
| /depts/** | /depts下的任意级路径 | 能匹配/depts，/depts/1，/depts/1/2，不能匹配/emps/1 |

#### 9.4.3 执行流程

![](img/拦截器执行流程.png)

- 当我们打开浏览器来访问部署在web服务器当中的web应用时，此时我们所定义的过滤器会拦截到这次请求。拦截到这次请求之后，它会先执行放行前的逻辑，然后再执行放行操作。而由于我们当前是基于springboot开发的，所以放行之后是进入到了spring的环境当中，也就是要来访问我们所定义的controller当中的接口方法。
- Tomcat并不识别所编写的Controller程序，但是它识别Servlet程序，所以在Spring的Web环境中提供了一个非常核心的Servlet：DispatcherServlet（前端控制器），所有请求都会先进行到DispatcherServlet，再将请求转给Controller。
- 当我们定义了拦截器后，会在执行Controller的方法之前，请求被拦截器拦截住。执行`preHandle()`方法，这个方法执行完成后需要返回一个布尔类型的值，如果返回true，就表示放行本次操作，才会继续访问controller中的方法；如果返回false，则不会放行（controller中的方法也不会执行）。
- 在controller当中的方法执行完毕之后，再回过来执行`postHandle()`这个方法以及`afterCompletion()` 方法，然后再返回给DispatcherServlet，最终再来执行过滤器当中放行后的这一部分逻辑的逻辑。执行完毕之后，最终给浏览器响应数据。

#### 9.4.4 Filter与Interceptor

过滤器和拦截器之间的区别：

- 接口规范不同：过滤器需要实现Filter接口，而拦截器需要实现HandlerInterceptor接口。
- 拦截范围不同：过滤器Filter会拦截所有的资源，而Interceptor只会拦截Spring环境中的资源

#### 9.4.5 登录校验

```java
@Slf4j
@Component
public class LoginCheckInterceptor implements HandlerInterceptor {

    @Override//在目标资源方法运行前执行，返回true，放行，返回false，不放行
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response,
                             Object handler) throws Exception {
        //1.获取请求url
        String uri = request.getRequestURI();
        log.info("请求的url:{}", uri);
        //2.判断请求url是否包含login，如果包含，说明是登陆操作，放行
        if(uri.contains("/login")){
            log.info("登录操作，放行...");
            return true;
        }
        //3.获取请求头中的令牌(token)
        String jwt = request.getHeader("token");
        //4.判断令牌是否存在，如果不存在，返回错误结果（未登录）
        if(!StringUtils.hasText(jwt)){
            log.info("请求头token为空，返回未登陆的信息");
            Result error = Result.error("NOT_LOGIN");
            //手动转换，将对象转换为json格式的对象------》alibabaJSON tool jar
            String noLogin = JSONObject.toJSONString(error);
            response.getWriter().write(noLogin);
            return false;
        }
        //5.解析token，如果解析失败，返回错误结果
        try {
            JwtUtils.parseJWT(jwt);
        } catch (Exception e) {//jwt解析失败
            log.error("error:{}", e.getMessage());
            log.info("解析令牌失败，但未登陆的错误信息");
            Result error = Result.error("NOT_LOGIN");
            //手动转换，将对象转换为json格式的对象------》alibabaJSON tool jar
            String noLogin = JSONObject.toJSONString(error);
            response.getWriter().write(noLogin);
            return false;
        }

        //放行
        log.info("令牌合法，放行");
        return true;

    }

    @Override//目标资源方法运行后执行
    public void postHandle(HttpServletRequest request, HttpServletResponse response,
                           Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("postHandle...");

    }

    @Override//视图渲染完毕后运行，最后运行
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response,
                                Object handler, Exception ex) throws Exception {
        System.out.println("afterCompletion...");
    }
}
```

### 9.5 异常处理

在三层构架项目中，出现了异常，如何处理：

- 方案一：在所有Controller的所有方法中进行try…catch处理
  - 缺点：代码臃肿（不推荐）
- 方案二：全局异常处理器
  - 好处：简单、优雅（推荐）

全局异常处理器：

- 定义全局异常处理器非常简单，就是定义一个类，在类上加上一个注解@RestControllerAdvice，加上这个注解就代表我们定义了一个全局异常处理器。
- 在全局异常处理器当中，需要定义一个方法来捕获异常，在这个方法上需要加上注解@ExceptionHandler。通过@ExceptionHandler注解当中的value属性来指定我们要捕获的是哪一类型的异常。

```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    //处理异常
    @ExceptionHandler(Exception.class) //指定能够处理的异常类型
    public Result ex(Exception e){
        e.printStackTrace();//打印堆栈中的异常信息

        //捕获到异常之后，响应一个标准的Result
        return Result.error("对不起,操作失败,请联系管理员");
    }
}
```

> @RestControllerAdvice = @ControllerAdvice + @ResponseBody
>
> 处理异常的方法返回值会转换为json后再响应给前端

以上就是全局异常处理器的使用，主要涉及到两个注解：

- @RestControllerAdvice  //表示当前类为全局异常处理器
- @ExceptionHandler  //指定可以捕获哪种类型的异常进行处理

