### MySQL技术内幕



### 第1章 MySQL体系结构和存储引擎

#### 1.1 定义数据库和实例

**MySQL数据库实例在系统上的表现就是一个进程**



#### 1.2 MySQL体系结构

理解数据库和数据库实例：

**数据库**是文件的集合，依照某种数据模型组织起来并存放于二级存储器中的数据集合。

二级存储器，指硬盘，固态硬盘。

一级存储器是主存。



**数据库实例**是位于用户和操作系统之间的一层数据管理软件，用户对数据库数据的任何操作都是在数据库实例下进行的。

应用程序只有通过数据库实例才能和数据库打交道。

所以，MySQL其实是数据库实例？（思考）

好吧，其实在实际中不会咬文嚼字，但用户一般都简单的把MySQL理解成数据库依旧有失偏颇。



![图1.1 MySQL体系结构](https://github.com/thiswv/Mysql_Insider/blob/main/images/%E5%9B%BE1.1%20MySQL%E4%BD%93%E7%B3%BB%E7%BB%93%E6%9E%84.png?raw=true)

​						*图1.1 MySQL体系结构*

MySQL区别其他数据库的重要特点是**可插拔式的表存储引擎。**

存储引擎是基于表的，而不是数据库，他是底层物理结构的实现。



#### 1.3 MySQL存储引擎（开源的）

存储引擎的好处是每个存储引擎都有各自的特点，能够根据具体的应用建立不通的存储引擎表。

**MySQL的核心在于存储引擎**



##### 1.3.1 InnoDB存储引擎

InnoDB存储引擎支持事务，设计目标面向在线事务处理（OLTP）的应用。

其特点是行锁设计，支持外键，支持非锁定读，即默认读取操作不会产生锁。



其余存储引擎了解，用到时再看。



**什么叫事务**，事务时MySQL区别其他传统的文件系统的一大特征。

事务指程序中进行一系列严密的逻辑操作，而且所有操作必须全部成功完成才算成功，否则会将所有操作撤销回滚。



#### 1.5 连接MySQL

连接MySQL操作是一个连接进程和MySQL数据库实例进行通信，本质是进程通信问题。



##### 1.5.1 TCP/IP

任何平台下都提供的连接方式，网络中使用最多的一种方式

```mysql
C:>mysql -h192.168.255.255 -u root -p
```

在windows下连接IP为192.168.255.255的Linux服务器。

Linux服务器会有一张权限试图，用来判断请求的客户端IP是否合法。

该视图在MySQL架构下，表明为user。



还有管道连接，UNIX连接，因为时间紧迫，跳过，用到时在细看。



### 第2章 InnoDB存储引擎

#### 2.1 InnoDB存储引擎概述

特点：

1. 行锁设计、支持MVCC、支持外键、提供一致性非锁定读
2. 高性能、高可用、高可扩展



概念先了解，虽然很多我都不清楚，但请我自己先学会放下，我相信作者在后续的章节中会一一解释。



***行锁设计***

**概念**：行锁是针对表中的某一行数据进行的锁定。当一个事务需要修改某一行数据时，它会请求并获得该行的行锁，其他事务需要修改相同行时必须等待。

**粒度**：只锁定单独的一行数据

**适用情景**：适用高并发的程序，最大程度减少锁的竞争，提高并发性



***MVCC***

全名**多版本并发控制**，多版本指的是数据库中同时存在多个版本的数据。

目的：为了提高数据库并发性能，用更好的方式去处理读-写冲突，做到即使有独写冲突，但也不用加上锁



***非锁定读***

默认读取操作不会加锁



#### 2.3 InnoDB体系结构

![图2.2 InnoDB体系结构](https://raw.githubusercontent.com/thiswv/Mysql_Insider/main/images/%E5%9B%BE2.1%20InnoDB%E4%BD%93%E7%B3%BB%E7%BB%93%E6%9E%84.bmp)

​						*图2.2 InnoDB体系结构*



