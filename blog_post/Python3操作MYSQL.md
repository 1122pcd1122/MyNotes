---
title: Python3操作MYSQL
categories:
  - Python
tags:
  - 数据库
  - python
  - Mysql
date: 2020-07-12 00:00:00
---
# Python3操作MYSQL

## PyMySQL安装

> PyMySQL 是在 Python3.x 版本中用于连接 MySQL 服务器的一个库

```python
pip install PyMySQL
```

## mysql连接

```python
connection = pymysql.connect(host='localhost', 
                             port=3306,
                             user='root',
                             password='peichendong17',
                             database='test',
                             charset='utf8'
)
```

|        参数        |                           描述                           |
| :----------------: | :------------------------------------------------------: |
|        host        |               数据库服务地址,默认localhost               |
|        user        |              用户名,默认为当前程序运行用户               |
|      password      |                 登录密码,默认为空字符串                  |
|        port        |                  数据库端口,默认为3306                   |
|      database      |                         数据库名                         |
|    cursorclass     |                    设置默认的游标类型                    |
|    read_timeout    |            读取数据超时时间,单位秒,默认无限制            |
|   write_timeout    |            写入数据超时时间,单位秒,默认无限制            |
|      charset       |                        数据库编码                        |
|    init_command    |          当连接建立完成之后执行的初始化SQL语句           |
| connection_timeout |          连接超时时间,默认10,最小1,最大3153600           |
|     autocommit     | 是否自动提交,默认不自动提交,参数值为None表示以服务器为准 |
| max_allowed_packet |           发送给服务器的最大数据量,默认为16MB            |
|   defer_connect    |               是否惰性连接,默认为立即连接                |
|         db         |                    已弃用的数据库别名                    |
|       passwd       |                  已弃用的数据库密码别名                  |
|                    |                                                          |

## 执行SQL

- cursor.execute(sql,args)执行单条SQL

  ```python
  # 获取游标
  cursor = connection.cursor()
  
  # 创建数据表
  effect_row = cursor.execute('''
  CREATE TABLE `users` (
    `name` varchar(32) NOT NULL,
    `age` int(10) unsigned NOT NULL DEFAULT '0',
    PRIMARY KEY (`name`)
  ) ENGINE=InnoDB DEFAULT CHARSET=utf8
  ''')
  
  # 插入数据(元组或列表)
  effect_row2 = cursor.execute('INSERT INTO `users` (`name`, `age`) VALUES (%s, %s)', ('mary', 18))
  
  info = {'name': '20', 'age': '18'}
  effect_row3 = cursor.execute('INSERT INTO users (name, age) VALUES (%(name)s, %(age)s)', info)
  ```

  

- executemany(sql,args)批量执行SQL

  ```python
  # 批量插入
  effect_row = cursor.executemany(
      'INSERT INTO `users` (`name`, `age`) VALUES (%s, %s) ON DUPLICATE KEY UPDATE age=VALUES(age)', [
          ('hello', 13),
          ('fake', 28),
      ])
  connection.commit()
  ```

  注意:INSERT,UPDATE,DELETE等修改数据的语句需手动执行connection.commit()完成对数据修改的提交

  ## 查询数据

  ```python
  cursor.execute('SELECT * FROM users')
  # # 当前位置是指游标当前所指的位置后移动一位
  # cursor.scroll(1, mode='relative')  # 相对当前位置移动
  # # 绝对位置是游标移动到参数中设置的位置
  # cursor.scroll(2, mode='absolute')  # 相对绝对位置移动
  # 单条数据
  print('返回游标所指的下一行数据:', cursor.fetchone())
  # 获取前N条数据
  print('游标所指位置的下两行数据:', cursor.fetchmany(2))
  ```
# 获取所有数据
  print('游标所指位置的剩余所有数据:', cursor.fetchall())
  # 上述三条每执行一次,会使游标向后移动
  ```
  
  

## 游标控制

​```python
# 当前位置是指游标当前所指的位置后移动一位
cursor.scroll(1, mode='relative')  # 相对当前位置移动
# 绝对位置是游标移动到参数中设置的位置
cursor.scroll(2, mode='absolute')  # 相对绝对位置移动
  ```

## 设置游标类型

查询时,默认返回的数据类型时元组,可以自定义设置返回类型,支持5中游标类型:

- Cursor:默认,元组类型

  ```python
  返回一条数据: ('20', 18)
  返回两条数据: (('fake', 28), ('hello', 13))
  返回所有数据 (('mary', 18),)
  
  ```

  

- DictCursor:字典类型

  ```python
  返回游标所指的下一行数据: {'name': '20', 'age': 18}
  游标所指位置的下两行数据: [{'name': 'fake', 'age': 28}, {'name': 'hello', 'age': 13}]
  游标所指位置的剩余所有数据: [{'name': 'mary', 'age': 18}]
  ```

  

- DictCursorMixin:支持自定义的游标类型,需自定义才可以使用

- SSCursor:无缓冲元组类型

- SSDictCursor:无缓冲字典类型

无缓冲游标类型,适用于数据量很大 ,一次性返回太慢,或者服务端宽较小时.

## 事务处理

- 开启事务`connection.begin()`
- 提交事务`connection.commit()`
- 回滚事务`connection.rollback()`

## 参考链接

[PyMySQL的API参考文档](https://pymysql.readthedocs.io/en/latest/modules/index.html)