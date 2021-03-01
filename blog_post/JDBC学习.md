---
title:JDBC学习
tags:
	-数据库
	-java
	-Mysql
date:2020-07-12 00:00:00
---

# JDBC学习

## JDBC的基本概念

​		概念:Java DataBase Connectivity Java数据库连接,Java语言操作数据库

> ​		JDBC本质:其实是官方定义的一套操作所有关系型数据库的规则,即接口.各个数据库厂商去实现这个接口,提供数据库驱动jar包.我们可以使用这套接口(JDBC)编程,真正执行的代码是驱动jar包中的实现类

## 快速入门

### 步骤:

1. .导入驱动jar包
                                1.复制C:\java\mysql-connector-java-8.0.20\mysql-connector-java-8.0.20.jar到项目的libs目录下
                                 2.右键--->Add As Library

2. 注册驱动

3. 获取数据库连接对象

4. 定义sql

5. 获取执行sql语句的对象Statement

6. 执行sql接收返回结果

7. 处理结果

8. 释放资源

   ```java
   	    //1.导入驱动jar包
           //2.注册驱动
           Class.forName("com.mysql.cj.jdbc.Driver");
           //3.获取数据库连接对象
           Connection connection =DriverManager.getConnection("jdbc:mysql://localhost:3306/04_mysql?useSSL=false&allowPublicKeyRetrieval=true&serverTimezone=UTC",USER,PASS);
           //4.定义sql语句
           String sql="update bank set bankNo = 464861 where id = 1";
           //5.获取执行sql对象 Statement
           Statement statement=connection.createStatement();
           //6.执行sql
           int count=statement.executeUpdate(sql);
           //处理结果
           System.out.println(count);
           //释放资源
           statement.close();
           connection.close();
   ```

   ```java
   //导入jar包   
   
           Connection connection = null;
           ResultSet resultSet=null;
           PreparedStatement preparedStatement=null;
           try {
               //注册驱动,获取连接
               connection = JdbcUtils.getConnection();
               //定义SQL
               String sql="select * from user where username=? and password=? ";
               //获取执行对象
                preparedStatement=connection.prepareStatement(sql);
   
               //result指的是所求数据节点的上一个位置
               //给？赋值
               preparedStatement.setString(1,username);
               preparedStatement.setString(2,password);
               //执行
               resultSet=preparedStatement.executeQuery();
   
               return resultSet.next();
   
           } catch (SQLException throwables) {
               throwables.printStackTrace();
           }finally {
               JdbcUtils.close(resultSet,preparedStatement,connection);
           }
   ```

   

## 详解各个对象

-  DriverManger:驱动管理对象

  > 用于管理一组JDBC驱动服务的基本程序

  > 1. 注册驱动:
  >
  >    ​		static void registerDriver(Driver driver):注册于给定的驱动程序的DriverManger
  >
  >    ​		写代码使用: Class.forName("com.mysql.cj.jdbc.Driver");
  >
  >    注意:mysql5之后的驱动jar包可以省略驱动注册驱动的步骤
  >
  >    

  > ​	2.获取数据库连接:
  >
  > ​		方法:static Connection getConnection(String url,String user,String password)
  >
  > ​		参数:
  >
  > ​				*url:指定连接的路径:
  >
  > ```java
  > /*locahost:IP地址 3306:本机的端口号 test_demo:库名 useSSL:MySQL 8.0 以上版本不需要建立 SSL 连接的,需要显示关闭 allowPublicKeyRetrieval=true 允许客户端从服务器获取公钥。serverTimezone=UTC:指定时区(UTC代表的是全球标准时间)*/
  > //语法:jdbc:mysql:IP地址(域名):端口号/数据库名称?SSL设置关闭&客户端获取公钥=设置指定时区
  > jdbc:mysql://localhost:3306/test_demo?useSSL=false&allowPublicKeyRetrieval=true&serverTimezone=UTC
  > ```
  >
  > ​				*user:
  >
  > ```java
  > 用户名--->root 
  > ```
  >
  > ​				*password:
  >
  > ```java
  > 密码--->password
  > ```
  >
  > 

-  Connection:数据库连接对象

  > 功能:
  >
  > ​		1.获取执行sql的对象
  >
  > ​			*Statement createStatement();
  >
  > ​			*PreparedStatement prepareStatement(String url) 创建一个PrepareStatement对象,用于将参数sql语句发送至数据库
  >
  > ​		 2.开启事务
  >
  > ​			 *开启事务: void setAutoCommit(boolean autoCommit)  :调用该方法设置参数为false,即开启事务
  >
  > ​			 *提交事务:commit();
  >
  > ​			 *回滚事务:rollback();

- Statement:执行sql的对象

  > 1.执行sql
  >
  > ​			1.bolean execute(String url):可以执行任意的slq 
  >
  > ​			2.int executeUpdate(String url): 执行DML(insert update delete)语句,DDL(create alter drop)语句
  >
  > ​				*返回值:影响的行数,可以通过影响的行数判断DML语句是否执行成功,返回值>0则执行成功,否则则执行失败
  >
  > ​			3.ResuleSet executeQuery(String sql) 执行DML(select)语句

- ResultSet:

  > 结果集对象,封装查询的结果
  >
  > next()：游标向下一行移动
  >
  > getxxx(): 获取数据
  >
  > ​		xxx代表数据类型 如:int getInt()，String getString()
  >
  > ​		参数:int 代表列的编号 从1开始 	如:getString(1)
  >
  > ​				String 代表列名称				 如:getString("price")

- ProparddStatement:执行sql的对象 
                  

## 抽取JDBC工具类: JDBCUtils

### 目的:简化书写

### 分析:

1. 注册驱动也抽取

2. 抽取一个方法获取连接对象

   1. 需求:不想传递参数(麻烦),还得保证工具类的通用性

   2. 解决:配置文件

      ​	jdbc.properties

      ​	url=

      ​	user=

      ​	password=

      ​	Driver=

      ```java
      url=jdbc:mysql://localhost:3306/04_mysql?useSSL=false&allowPublicKeyRetrieval=true&serverTimezone=UTC
      user=root
      password=root
      driver=com.mysql.cj.jdbc.Driver
      ```

      

3.抽取一个方法释放资源

### 实现:

```java
import javax.swing.plaf.nimbus.State;
import java.io.FileReader;
import java.io.IOException;
import java.net.URL;
import java.sql.*;
import java.util.Properties;

/**
 * @author peichendong
 */
public class JdbcUtils {

    private static String url;
    private static String user;
    private static String password;

    /*
    * 文件的读取,只需要读取一次即可拿到这些值,使用静态代码块
    * */
    static {
        //读取资源文件

        //加载文件
        try {
            //1.创建Properties集合类
            Properties properties=new Properties();
            //获取src路径下的文件的方式--->ClassLoader类加载器
            ClassLoader classLoader=JdbcUtils.class.getClassLoader();
            URL res=classLoader.getResource("src/jdbc.properties");
            String path=res.getPath();
            properties.load(new FileReader(path));

            url=properties.getProperty("url");
            user=properties.getProperty("user");
            password=properties.getProperty("password");
            String driver = properties.getProperty("driver");
            try {
                Class.forName(driver);
            } catch (ClassNotFoundException e) {
                e.printStackTrace();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }

    }

    /**
     * @return 连接对象
     */
    public static Connection getConnection() throws SQLException {
        return DriverManager.getConnection(url,user,password);
    }

    /**
     * 释放资源
     * @param statement
     * @param connection
     */
    public static void close(Statement statement,Connection connection){
        if (statement!=null){
            try {
                statement.close();
            }catch (SQLException e){
                e.printStackTrace();
            }
        }

        if (connection!=null){
            try {
                connection.close();
            }catch (SQLException e){
                e.printStackTrace();
            }
        }
    }

    /**
     * 释放资源
     * @param resultSet
     * @param statement
     * @param connection
     */
    public static void close(ResultSet resultSet, Statement statement, Connection connection){

        if (resultSet!=null){
            try {
                resultSet.close();
            }catch (SQLException e){
                e.printStackTrace();
            }
        }

        if (statement!=null){
            try {
                statement.close();
            }catch (SQLException e){
                e.printStackTrace();
            }
        }

        if (connection!=null){
            try {
                connection.close();
            }catch (SQLException e){
                e.printStackTrace();
            }
        }
    }
}
```

## JDBC控制事务

### 	1.事务

> 事务:一个包含多个步骤的业务操作,如果这个业务被事务管理,则这多个步骤要么同时成功,要么同时失败

### 	2.操作

1. 开启事务
2. 提交事务
3. 回滚事务

### 3.使用connection对象来管理事务

- 开启事务:setAutoCommit(boolean autoCommit):调用该方法设置参数为false,即开启事务
  - 在执行sql之前开启事务
- 提交事务:commit()
  - 当所有sql都执行之后提交事务
- 回滚事务:rollback()
  - 在catch中回滚事务

## 数据库连接池

1. 概念:其实就是一个容器(集合),存放数据库连接的容器

   1. 当系统初始化好后,容器被创建,当用户来访问数据库时,从容器中获取连接对象,用户访问完之后,将连接对象归还给容器。

2. 好处:

   1. 节约资源
   2. 用户访问高效

3. 实现:

   1. 标准接口:DataSource 		java.sql包下

      1. 方法:
         - 获取连接:getConnection()
         - 归还连接:如果连接对象Connection是从连接池中获取的,那么调用Connection.close()方法,咋不会关闭连接,而是归还连接

   2. 一般我们不去实现它,有数据库厂商来实现

      1. C3P0:数据库连接技术
      2. Druid:数据库连接池实现技术,由阿里巴巴提供的

   3. 

      

4. 1. - - 

5. 

   

