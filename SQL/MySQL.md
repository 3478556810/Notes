# 前言

在我们讲解SpringBootWeb基础知识(请求响应案例)的时候，我们讲到在web开发中，为了应用程序职责单一，方便维护，我们一般将web应用程序分为三层，即：Controller、Service、Dao 。

之前我们的案例中，是这样子的请求流程：浏览器发起请求，先请求Controller；Controller接收到请求之后，调用Service进行业务逻辑处理；Service再调用Dao，Dao再解析user.xml中所存储的数据。

![image-20221205001241294](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20221205001241294.png?x-os)

xml文件中可以存储数据，但是在企业项目开发中不会使用xml文件存储数据，因为不便管理维护，操作难度大。 在真实的企业开发中呢，都会采用数据库来存储和管理数据，那此时，web开发调用流程图如下所示：

![image-20221205001346266](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20221205001346266.png?x-os)

首先来了解一下什么是数据库。

- 数据库：英文为 DataBase，简称DB，它是存储和管理数据的仓库。

像我们日常访问的电商网站京东，企业内部的管理系统OA、ERP、CRM这类的系统，以及大家每天都会刷的头条、抖音类的app，那这些大家所看到的数据，其实都是存储在数据库中的。最终这些数据，只是在浏览器或app中展示出来而已，最终数据的存储和管理都是数据库负责的。

数据是存储在数据库中的，那我们要如何来操作数据库以及数据库中所存放的数据呢？

那这里呢，会涉及到一个软件：数据库管理系统（**D**ata**B**ase **M**anagement **S**ystem，简称DBMS）

- DBMS是操作和管理数据库的大型软件。将来我们只需要操作这个软件，就可以通过这个软件来操纵和管理数据库了。

此时又出现一个问题：DBMS这个软件怎么知道要操作的是哪个数据库、哪个数据呢？是对数据做修改还是查询呢？

- 需要给DBMS软件发送一条指令，告诉这个软件我们要执行的是什么样的操作，要对哪个数据进行操作。而这个指令就是SQL语句

SQL（**S**tructured **Q**uery **L**anguage，简称SQL）：结构化查询语言，它是操作关系型数据库的编程语言，定义了一套操作关系型数据库的统一标准。我们学习数据库开发，最为重要的就是学习SQL语句 。

> 关系型数据库：我们后面会详细讲解，现在大家只需要知道我们学习的数据库属于关系型数据库即可。



结论：程序员给数据库管理系统(DBMS)发送SQL语句，再由数据库管理系统操作数据库当中的数据。

了解了数据库的一些简单概念之后，接下来我们再来介绍下目前主流的数据库，这里截取了排名前十的数据库：

- Oracle：大型的收费数据库，Oracle公司产品，价格昂贵。（通常是不差钱的公司会选择使用这个数据库）
- MySQL：开源免费的中小型数据库，后来Sun公司收购了MySQL，而Oracle又收购了Sun公司。目前Oracle推出两个版本的Mysql：社区版(开源免费)、商业版(收费)。
- SQL Server：Microsoft 公司推出的收费的中型数据库，C#、.net等语言常用。
- PostgreSQL：开源免费的中小型数据库。
- DB2：IBM公司的大型收费数据库产品。
- SQLLite：嵌入式的微型数据库。Android内置的数据库采用的就是该数据库。
- MariaDB：开源免费的中小型数据库。是MySQL数据库的另外一个分支、另外一个衍生产品，与MySQL数据库有很好的兼容性。

那这么多数据库，我们全部都需要学习吗，其实并不用，我们只需要学习其中的一个就可以了，我们此次课程中学习的数据库是现在互联网公司开发使用最为流行的MySQL数据库。

此时大家可能会有一个疑问，我们现在学习的是Mysql数据库，我们以后去公司做开发，如果用到的是Oracle数据库或SQL Server数据库该怎么办？其实大家完全不用担心这个问题，因为这些数据库都是属于关系型数据库，要操作关系型数据库都是通过 SQL语句来实现的，而SQL语句又是操作关系型数据库的统一标准。

> 结论：只要我们学会了SQL语句，就可以通过SQL语句来操作Mysql，也可以通过SQL语句来操作Oracle或SQL Server

我们将学习MySQL内容拆解为3部分知识点：

![image-20221205122937131](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20221205122937131.png?x-os)



# 1. MySQL概述

![image-20220610191829748](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20220610191829748.png?x-os) 

官网：https://dev.mysql.com/

### 1.1 Windows安装

#### 1.1.1 版本

MySQL官方提供了两个版本：

- 商业版本（MySQL Enterprise Edition）
  - 该版本是收费的，我们可以使用30天。 官方会提供对应的技术支持。

- 社区版本（MySQL Community Server）
  - 该版本是免费的，但是MySQL不会提供任何的技术支持。

> 本课程，采用的是MySQL的社区版本（8.0.31）



#### 1.1.2 安装

官网下载地址：https://downloads.mysql.com/archives/community/

![image-20221205140643412](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20221205140643412.png?x-os)



#### 1.1.3 连接

- MySQL服务器启动完毕后，输入以下指令进行MySQL的初始化

![image-20230406095344365](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406095344365.png?x-os)

- 注册MySQL服务

![image-20230406095635378](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406095635378.png?x-os)

- 启动MySQL

  ![](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406095911102.png?x-os)

- 修改默认账户密码

  ![image-20230406100057535](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406100057535.png?x-os)

- 然后再使用如下指令，来连接MySQL服务器：

```bash
mysql -u用户名 -p密码 [-h数据库服务器的IP地址 -P端口号]
```

> -h  参数不加，默认连接的是本地 127.0.0.1 的MySQL服务器
>
> -P  参数不加，默认连接的端口号是 3306

**上述指令，可以有两种形式：**

- 密码直接在-p参数之后直接指定 （这种方式不安全，密码直接以明文形式出现在命令行）

  ![image-20230406100243615](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406100243615.png?x-os)

- 密码在-p回车之后，在命令行中输入密码，然后回车

  ![image-20230406100359471](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406100359471.png?x-os)

### 1.2 Linux操作

#### 1.2.1 连接

```bash
mysql -uroot -pyourPassword
```

#### 1.2.2 修改密码

```bash
use mysql
update user set password=password("yourPassword") where user="root";
```

#### 1.2.3 修改权限

```bash
update user set Host='%' where User='root' and host='localhost';
FLUSH PRIVILEGES;
```

该行指令使root用户可以外网访问数据库

### 1.3 数据模型

介绍完了Mysql数据库的安装配置之后，接下来我们再来聊一聊Mysql当中的数据模型。学完了这一小节之后，我们就能够知道在Mysql数据库当中到底是如何来存储和管理数据的。

在介绍 Mysql的数据模型之前，需要先了解一个概念：关系型数据库。

**关系型数据库（RDBMS）**

概念：建立在关系模型基础上，由多张相互连接的**二维表**组成的数据库。

二维表的优点：

- 使用表存储数据，格式统一，便于维护

- 使用SQL语言操作，标准统一，使用方便，可用于复杂查询

> 我们之前提到的MySQL、Oracle、DB2、SQLServer这些都是属于关系型数据库，里面都是基于二维表存储数据的。
>
> 结论：基于二维表存储数据的数据库就成为关系型数据库，不是基于二维表存储数据的数据库，就是非关系型数据库（比如大家后面要学习的Redis，就属于非关系型数据库）。



**2). 数据模型**

介绍完了关系型数据库之后，接下来我们再来看一看在Mysql数据库当中到底是如何来存储数据的，也就是Mysql 的数据模型。

MySQL是关系型数据库，是基于二维表进行数据存储的，具体的结构图下:

![image-20220829111741419](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20220829111741419.png?x-os) 

- 通过MySQL客户端连接数据库管理系统DBMS，然后通过DBMS操作数据库
- 使用MySQL客户端，向数据库管理系统发送一条SQL语句，由数据库管理系统根据SQL语句指令去操作数据库中的表结构及数据
- 一个数据库服务器中可以创建多个数据库，一个数据库中也可以包含多张表，而一张表中又可以包含多行记录。

> 在Mysql数据库服务器当中存储数据，你需要：
>
> 1. 先去创建数据库（可以创建多个数据库，之间是相互独立的）
> 2. 在数据库下再去创建数据表（一个数据库下可以创建多张表）
> 3. 再将数据存放在数据表中（一张表可以存储多行数据）





### 1.4 SQL简介

SQL：结构化查询语言。一门操作关系型数据库的编程语言，定义操作所有关系型数据库的统一标准。

在学习具体的SQL语句之前，先来了解一下SQL语言的语法。

#### 1.4.1 SQL通用语法

1、SQL语句可以单行或多行书写，以分号结尾。

2、SQL语句可以使用空格/缩进来增强语句的可读性。

3、MySQL数据库的SQL语句不区分大小写。

4、注释：

- 单行注释：-- 注释内容   或   # 注释内容(MySQL特有)
- 多行注释： /* 注释内容 */

> 以上就是SQL语句的通用语法，这些通用语法大家目前先有一个直观的认识，我们后面在讲解每一类SQL语句的时候，还会再来强调通用语法。



#### 1.4.2 分类

SQL语句根据其功能被分为四大类：DDL、DML、DQL、DCL 

| **分类** | **全称**                    | **说明**                                               |
| -------- | --------------------------- | ------------------------------------------------------ |
| DDL      | Data Definition  Language   | 数据定义语言，用来定义数据库对象(数据库，表，字段)     |
| DML      | Data Manipulation  Language | 数据操作语言，用来对数据库表中的数据进行增删改         |
| DQL      | Data Query Language         | 数据查询语言，用来查询数据库中表的记录                 |
| DCL      | Data Control  Language      | 数据控制语言，用来创建数据库用户、控制数据库的访问权限 |



# 2. 数据库设计-DDL

下面我们就正式的进入到SQL语句的学习，在学习之前先给大家介绍一下我们要开发一个项目，整个开发流程是什么样的，以及在流程当中哪些环节会涉及到数据库。

### 2.1 项目开发流程

![image-20220829112953742](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20220829112953742.png?x-os)

需求文档：

- 在我们开发一个项目或者项目当中的某个模块之前，会先会拿到产品经理给我们提供的页面原型及需求文档。

![image-20221205154101142](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20221205154101142.png?x-os)

设计：

- 拿到产品原型和需求文档之后，我们首先要做的不是编码，而是要先进行项目的设计，其中就包括概要设计、详细设计、接口设计、数据库设计等等。
- 数据库设计根据产品原型以及需求文档，要分析各个模块涉及到的表结构以及表结构之间的关系，以及表结构的详细信息。最终我们需要将数据库以及数据库当中的表结构设计创建出来。

开发/测试：

- 参照页面原型和需求进行编码，实现业务功能。在这个过程当中，我们就需要来操作设计出来的数据库表结构，来完成业务的增删改查操作等。

部署上线：

- 在项目的功能开发测试完成之后，项目就可以上线运行了，后期如果项目遇到性能瓶颈，还需要对项目进行优化。优化很重要的一个部分就是数据库的优化，包括数据库当中索引的建立、SQL 的优化、分库分表等操作。



在上述的流程当中，针对于数据库来说，主要包括三个阶段：

1. 数据库设计阶段
   - 参照页面原型以及需求文档设计数据库表结构
2. 数据库操作阶段
   - 根据业务功能的实现，编写SQL语句对数据表中的数据进行增删改查操作
3. 数据库优化阶段
   - 通过数据库的优化来提高数据库的访问性能。优化手段：索引、SQL优化、分库分表等

接下来我们就先来学习第一部分数据库的设计，而数据库的设计就是来定义数据库，定义表结构以及表中的字段。



### 2.2 数据库操作

我们在进行数据库设计，需要使用到刚才所介绍SQL分类中的DDL语句。

DDL英文全称是Data Definition Language(数据定义语言)，用来定义数据库对象(数据库、表)。

DDL中数据库的常见操作：查询、创建、使用、删除。

#### 2.2.1 查询数据库

**查询所有数据库：**

```mysql
show databases;
```

命令行中执行效果如下：![image-20230406102212023](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406102212023.png?x-os)

**查询当前数据库：**

```mysql
select database();
```

命令行中执行效果如果：

![image-20230406102243159](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406102243159.png?x-os)



#### 2.2.2 创建数据库

**语法：**

```mysql
create database [ if not exists ] 数据库名;
```

案例： 创建一个itcast数据库。

```mysql
create database RNG;
```

命令行执行效果如下：

![image-20230406102447443](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406102447443.png?x-os)

注意：在同一个数据库服务器中，不能创建两个名称相同的数据库，否则将会报错。

- 可以使用if not exists来避免这个问题

```sql
-- 数据库不存在,则创建该数据库；如果存在则不创建
create database if not extists RNG; 
```

#### 2.2.3 使用数据库

**语法：**

```mysql
use 数据库名 ;
```

> 我们要操作某一个数据库下的表时，就需要通过该指令，切换到对应的数据库下，否则不能操作。

案例：切换到itcast数据

```mysql
use RNG;
```

命令执行效果如下：

![image-20230406102635900](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406102635900.png?x-os)

#### 2.2.4 删除数据库

**语法：**

```mysql
drop database [ if exists ] 数据库名 ;
```

> 如果删除一个不存在的数据库，将会报错。
>
> 可以加上参数 if exists ，如果数据库存在，再执行删除，否则不执行删除。

案例：删除itcast数据库

~~~mysql
drop database if exists RNG; 
~~~

命令执行效果如下：

![image-20230406102736771](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406102736771.png?x-os)

说明：上述语法中的database，也可以替换成 schema

- 如：create schema db01;
- 如：show schemas;

### 2.3 图形化工具

#### 2.3.1 介绍

前面我们讲解了DDL中关于数据库操作的SQL语句，在我们编写这些SQL时，都是在命令行当中完成的。大家在练习的时候应该也感受到了，在命令行当中来敲这些SQL语句很不方便，主要的原因有以下 3 点：

1. 没有任何代码提示。（全靠记忆，容易敲错字母造成执行报错）
2. 操作繁琐，影响开发效率。（所有的功能操作都是通过SQL语句来完成的）
3. 编写过的SQL代码无法保存。

在项目开发当中，通常为了提高开发效率，都会借助于现成的图形化管理工具来操作数据库。

#### 2.3.2 安装

> 说明：DataGrip这款工具可以不用安装，因为Jetbrains公司已经将DataGrip这款工具的功能已经集成到了 IDEA当中，所以我们就可以使用IDEA来作为一款图形化界面工具来操作Mysql数据库。



#### 2.3.3 使用

- 使用IDEA自带的Database

![image-20230406103616223](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406103616223.png?x-os)

- 使用DDL语句，展示所有数据库对象

  ![image-20230406103809483](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406103809483.png?x-os)





### 2.4  表操作

学习完了DDL语句当中关于数据库的操作之后，接下来我们继续学习DDL语句当中关于表结构的操作。

关于表结构的操作也是包含四个部分：创建表、查询表、修改表、删除表。

#### 2.4.1 创建

##### 2.4.1.1 语法

```mysql
create table  表名(
	字段1  字段1类型 [约束]  [comment  字段1注释 ],
	字段2  字段2类型 [约束]  [comment  字段2注释 ],
	......
	字段n  字段n类型 [约束]  [comment  字段n注释 ] 
) [ comment  表注释 ] ;
```

> 注意： [ ] 中的内容为可选参数； 最后一个字段后面没有逗号

- 建表语句： 

```mysql
create table player(
    id int  comment 'LPL编号',
    name varchar(10)  comment '选手名',
    age int  comment  '选手年龄',
    gender char(1)  comment '选手性别'
)comment 'RNG 选手表'
```

> 数据表创建完成，接下来我们还需要测试一下是否可以往这张表结构当中来存储数据。

![image-20230406105852886](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406105852886.png?x-os)

我们之前提到过：id字段是一行数据的唯一标识，不能有重复值。

- 其实我们现在创建表结构的时候， 在数据库层面呢，并没有去限制字段存储的数据。所以id这个字段没有起到唯一标识的作用。

> 想要限制字段所存储的数据，就需要用到数据库中的约束。





##### 2.4.1.2 约束

概念：所谓约束就是作用在表中字段上的规则，用于限制存储在表中的数据。

作用：就是来保证数据库当中数据的正确性、有效性和完整性。（后面的学习会验证这些）

在MySQL数据库当中，提供了以下5种约束：

| **约束** | **描述**                                         | **关键字**  |
| -------- | ------------------------------------------------ | ----------- |
| 非空约束 | 限制该字段值不能为null                           | not null    |
| 唯一约束 | 保证字段的所有数据都是唯一、不重复的             | unique      |
| 主键约束 | 主键是一行数据的唯一标识，要求非空且唯一         | primary key |
| 默认约束 | 保存数据时，如果未指定该字段值，则采用默认值     | default     |
| 外键约束 | 让两张表的数据建立连接，保证数据的一致性和完整性 | foreign key |

> 注意：约束是作用于表中字段上的，可以在创建表/修改表的时候添加约束。

- 建表语句： 

```mysql
create table player(
    id int primary key comment 'LPL编号',
    name varchar(10) not null comment '选手名',
    age int not null comment  '选手年龄',
    gender char(1) default '男' comment '选手性别'
)comment 'RNG 选手表'
```

大家有没有发现一个问题：id字段下存储的值，如果由我们自己来维护会比较麻烦(必须保证值的唯一性)。MySQL数据库为了解决这个问题，给我们提供了一个关键字：auto_increment（自动增长）

> 主键自增：auto_increment
>
> - 每次插入新的行记录时，数据库自动生成id字段(主键)下的值
> - 具有auto_increment的数据列是一个正数序列开始增长(从1开始自增)

~~~mysql
create table player(
    id int primary key auto_increment comment 'LPL编号',
    name varchar(10) not null comment '选手名',
    age int not null comment  '选手年龄',
    gender char(1) default '男' comment '选手性别'
)comment 'RNG 选手表'
~~~

![image-20230406110044628](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406110044628.png?x-os)

##### 2.4.1.3 数据类型

在上面建表语句中，我们在指定字段的数据类型时，用到了int 、varchar、char，那么在MySQL中除了以上的数据类型，还有哪些常见的数据类型呢？ 接下来,我们就来详细介绍一下MySQL的数据类型。

MySQL中的数据类型有很多，主要分为三类：数值类型、字符串类型、日期时间类型。

**数值类型**

| 类型        | 大小   | 有符号(SIGNED)范围                                    | 无符号(UNSIGNED)范围                                       | 描述               |
| ----------- | ------ | ----------------------------------------------------- | ---------------------------------------------------------- | ------------------ |
| TINYINT     | 1byte  | (-128，127)                                           | (0，255)                                                   | 小整数值           |
| SMALLINT    | 2bytes | (-32768，32767)                                       | (0，65535)                                                 | 大整数值           |
| MEDIUMINT   | 3bytes | (-8388608，8388607)                                   | (0，16777215)                                              | 大整数值           |
| INT/INTEGER | 4bytes | (-2147483648，2147483647)                             | (0，4294967295)                                            | 大整数值           |
| BIGINT      | 8bytes | (-2^63，2^63-1)                                       | (0，2^64-1)                                                | 极大整数值         |
| FLOAT       | 4bytes | (-3.402823466 E+38，3.402823466351 E+38)              | 0 和 (1.175494351  E-38，3.402823466 E+38)                 | 单精度浮点数值     |
| DOUBLE      | 8bytes | (-1.7976931348623157 E+308，1.7976931348623157 E+308) | 0 和  (2.2250738585072014 E-308，1.7976931348623157 E+308) | 双精度浮点数值     |
| DECIMAL     |        | 依赖于M(精度)和D(标度)的值                            | 依赖于M(精度)和D(标度)的值                                 | 小数值(精确定点数) |

```sql
示例: 
    年龄字段 ---不会出现负数, 而且人的年龄不会太大
	age tinyint unsigned
	
	分数 ---总分100分, 最多出现一位小数
	score double(4,1)
```

**字符串类型**

| 类型       | 大小                  | 描述                         |
| ---------- | --------------------- | ---------------------------- |
| CHAR       | 0-255 bytes           | 定长字符串(需要指定长度)     |
| VARCHAR    | 0-65535 bytes         | 变长字符串(需要指定长度)     |
| TINYBLOB   | 0-255 bytes           | 不超过255个字符的二进制数据  |
| TINYTEXT   | 0-255 bytes           | 短文本字符串                 |
| BLOB       | 0-65 535 bytes        | 二进制形式的长文本数据       |
| TEXT       | 0-65 535 bytes        | 长文本数据                   |
| MEDIUMBLOB | 0-16 777 215 bytes    | 二进制形式的中等长度文本数据 |
| MEDIUMTEXT | 0-16 777 215 bytes    | 中等长度文本数据             |
| LONGBLOB   | 0-4 294 967 295 bytes | 二进制形式的极大文本数据     |
| LONGTEXT   | 0-4 294 967 295 bytes | 极大文本数据                 |

char 与 varchar 都可以描述字符串，char是定长字符串，指定长度多长，就占用多少个字符，和字段值的长度无关 。而varchar是变长字符串，指定的长度为最大占用长度 。相对来说，char的性能会更高些。

```sql
示例： 
    用户名 username ---长度不定, 最长不会超过50
	username varchar(50)
	
	手机号 phone ---固定长度为11
	phone char(11)
```

**日期时间类型**

| 类型      | 大小 | 范围                                       | 格式                | 描述                     |
| --------- | ---- | ------------------------------------------ | ------------------- | ------------------------ |
| DATE      | 3    | 1000-01-01 至  9999-12-31                  | YYYY-MM-DD          | 日期值                   |
| TIME      | 3    | -838:59:59 至  838:59:59                   | HH:MM:SS            | 时间值或持续时间         |
| YEAR      | 1    | 1901 至 2155                               | YYYY                | 年份值                   |
| DATETIME  | 8    | 1000-01-01 00:00:00 至 9999-12-31 23:59:59 | YYYY-MM-DD HH:MM:SS | 混合日期和时间值         |
| TIMESTAMP | 4    | 1970-01-01 00:00:01 至 2038-01-19 03:14:07 | YYYY-MM-DD HH:MM:SS | 混合日期和时间值，时间戳 |

```sql
示例: 
	生日字段  birthday ---生日只需要年月日  
	birthday date
	
	创建时间 createtime --- 需要精确到时分秒
	createtime  datetime
```

#### 2.4.2 查询

> 关于表结构的查询操作，工作中一般都是直接基于**图形化界面操作**。 

**查询当前数据库所有表**

```mysql
show tables;
```

![image-20230406143809395](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406143809395.png?x-os)

**查看指定表结构**

```mysql
desc 表名 ;#可以查看指定表的字段、字段的类型、是否可以为NULL、是否存在默认值等信息
```

![image-20230406143923715](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406143923715.png?x-os)

**查询指定表的建表语句**

```mysql
show create table 表名 ;
```

![image-20230406144053611](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406144053611.png?x-os)

#### 2.4.3 修改

> 关于表结构的修改操作，工作中一般都是直接基于**图形化界面操作**。 

以下SQL语句仅作了解

**添加字段**

```sql
alter table 表名 add  字段名  类型(长度)  [comment 注释]  [约束];
```

**修改数据类型**

```mysql
alter table 表名 modify  字段名  新数据类型(长度);
```

```sql
alter table 表名 change  旧字段名  新字段名  类型(长度)  [comment 注释]  [约束];
```

**删除字段**

```sql
alter table 表名 drop 字段名;
```

**修改表名**

```sql
rename table 表名 to  新表名;
```

#### 2.4.4 删除

> 关于表结构的删除操作，工作中一般都是直接基于**图形化界面操作**。 

删除表语法：

```sql
drop  table [ if exists ]  表名;
```

> if exists ：只有表名存在时才会删除该表，表名不存在，则不执行删除操作(如果不加该参数项，删除一张不存在的表，执行将会报错)。

# 3. 数据库操作-DML

DML英文全称是Data Manipulation Language(数据操作语言)，用来对数据库中表的数据记录进行增、删、改操作。

- 添加数据（INSERT）
- 修改数据（UPDATE）
- 删除数据（DELETE） 

### 3.1 增加(insert)

insert语法：

- 向指定字段添加数据

  ~~~mysql
  insert into 表名 (字段名1, 字段名2) values (值1, 值2);
  ~~~

  ![image-20230406145705175](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406145705175.png?x-os)

- 全部字段添加数据

  ~~~mysql
  insert into 表名 values (值1, 值2, ...);
  ~~~

  ![image-20230406145912968](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406145912968.png?x-os)

- 批量添加数据（指定字段）

  ~~~mysql
  insert into 表名 (字段名1, 字段名2) values (值1, 值2), (值1, 值2);
  ~~~

- 批量添加数据（全部字段）

  ~~~mysql
  insert into 表名 values (值1, 值2, ...), (值1, 值2, ...);
  ~~~

![image-20230406150104203](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406150104203.png?x-os)



注意事项：

1. 插入数据时，指定的字段顺序需要与值的顺序是一一对应的。

2. 字符串和日期型数据应该包含在引号中。

3. 插入的数据大小，应该在字段的规定范围内。

### 3.2 修改(update)

update语法：

```sql
update 表名 set 字段名1 = 值1 , 字段名2 = 值2 , .... [where 条件] ;
```

![image-20230406150821465](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406150821465.png?x-os)



注意事项:

1. 修改语句的条件可以有，也可以没有，如果没有条件，则会修改整张表的所有数据。

2. 在修改数据时，一般需要同时修改公共字段update_time，将其修改为当前操作时间。



### 3.3 删除(delete)

delete语法：

```SQL
delete from 表名  [where  条件] ;
```

![image-20230406150925325](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406150925325.png?x-os)

注意事项:

​	• DELETE 语句的条件可以有，也可以没有，如果没有条件，则会删除整张表的所有数据。

​	• DELETE 语句不能删除某一个字段的值(可以使用UPDATE，将该字段值置为NULL即可)。

​	• 当进行删除全部数据操作时，会提示询问是否确认删除所有数据，直接点击Execute即可。 

# 4. 数据库查询-DQL

## 前言

在上次学习的内容中，我们讲解了：

- 使用DDL语句来操作数据库以及表结构（数据库设计）
- 使用DML语句来完成数据库中数据的增、删、改操作（数据库操作）

我们今天还是继续学习数据库操作方面的内容：查询（DQL语句）。

查询操作我们分为两部分学习：

- DQL语句-单表操作
- DQL语句-多表操作

DQL英文全称是Data Query Language(数据查询语言)，用来查询数据库表中的记录。

查询关键字：SELECT

查询操作是所有SQL语句当中最为常见，也是最为重要的操作。在一个正常的业务系统中，查询操作的使用频次是要远高于增删改操作的。当我们打开某个网站或APP所看到的展示信息，都是通过从数据库中查询得到的，而在这个查询过程中，还会涉及到条件、排序、分页等操作。

## 4.1 单表查询

### 4.1.1  语法

DQL查询语句，语法结构如下：

```mysql
SELECT
	字段列表
FROM
	表名列表
WHERE
	条件列表
GROUP  BY
	分组字段列表
HAVING
	分组后条件列表
ORDER BY
	排序字段列表
LIMIT
	分页参数
```

我们今天会将上面的完整语法拆分为以下几个部分学习：

- 基本查询（不带任何条件）
- 条件查询（where）
- 分组查询（group by）
- 排序查询（order by）
- 分页查询（limit）

### 4.1.2 基本查询

在基本查询的DQL语句中，不带任何的查询条件，语法如下：

- 查询多个字段

  ~~~mysql
  select 字段1, 字段2, 字段3 from  表名;
  ~~~

- 查询所有字段（通配符）

  ~~~mysql
  select *  from  表名;
  ~~~

- 设置别名

  ~~~mysql
  select 字段1 [ as 别名1 ] , 字段2 [ as 别名2 ]  from  表名;
  ~~~

- 去除重复记录

  ~~~mysql
  select distinct 字段列表 from  表名;
  ~~~



案例1：查询指定字段 name，entrydate并返回

~~~mysql
select name,entrydate from tb_emp;
~~~

![image-20230406152722185](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406152722185.png?x-os)



案例2：查询返回所有字段

~~~mysql
select * from tb_emp;
~~~

> `*`号代表查询所有字段，在实际开发中尽量少用（不直观、影响效率）

案例3：查询所有员工的 name,entrydate，并起别名(姓名、入职日期)

~~~mysql
-- 方式1：
select name AS 姓名, entrydate AS 入职日期 from tb_emp;
-- 方式2： 别名中有特殊字符时，使用''或""包含
select name AS '姓 名', entrydate AS '入职日期' from tb_emp;
-- 方式3：
select name AS "姓名", entrydate AS "入职日期" from tb_emp;
~~~



![image-20230406152818338](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406152818338.png?x-os)

案例4：查询已有的员工关联了哪几种职位(不要重复)

~~~mysql
select distinct job from tb_emp;
~~~

![image-20230406152905693](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406152905693.png?x-os)

### 4.1.3 条件查询

**语法：**

```sql
select  字段列表  from   表名   where   条件列表 ; -- 条件列表：意味着可以有多个条件
```

学习条件查询就是学习条件的构建方式，而在SQL语句当中构造条件的运算符分为两类：

- 比较运算符
- 逻辑运算符

常用的比较运算符如下: 

| **比较运算符**       | **功能**                                 |
| -------------------- | ---------------------------------------- |
| >                    | 大于                                     |
| >=                   | 大于等于                                 |
| <                    | 小于                                     |
| <=                   | 小于等于                                 |
| =                    | 等于                                     |
| <> 或 !=             | 不等于                                   |
| between ...  and ... | 在某个范围之内(含最小、最大值)           |
| in(...)              | 在in之后的列表中的值，多选一             |
| like 占位符          | 模糊匹配(_匹配单个字符, %匹配任意个字符) |
| is null              | 是null                                   |

常用的逻辑运算符如下:

| **逻辑运算符** | **功能**                    |
| -------------- | --------------------------- |
| and 或 &&      | 并且 (多个条件同时成立)     |
| or 或 \|\|     | 或者 (多个条件任意一个成立) |
| not 或 !       | 非 , 不是                   |



案例1：查询 姓名 为 杨逍 的员工

~~~mysql
select id, username, password, name, gender, image, job, entrydate, create_time, update_time
from tb_emp
where name = '杨逍'; -- 字符串使用''或""包含
~~~

![image-20230406153441636](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406153441636.png?x-os)

案例2：查询 id小于等于5 的员工信息

~~~mysql
select id, username, password, name, gender, image, job, entrydate, create_time, update_time
from tb_emp
where id <=5;
~~~

![image-20230406153542570](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406153542570.png?x-os)

案例3：查询 没有分配职位 的员工信息

~~~mysql
select id, username, password, name, gender, image, job, entrydate, create_time, update_time
from tb_emp
where job is null ;
~~~

![image-20230406153900922](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406153900922.png?x-os)

案例4：查询 密码不等于 '123456' 的员工信息

~~~mysql
-- 方式1：
select id, username, password, name, gender, image, job, entrydate, create_time, update_time
from tb_emp
where password <> '123456';
-- 方式2：
select id, username, password, name, gender, image, job, entrydate, create_time, update_time
from tb_emp
where password != '123456';
~~~

![image-20230406154042847](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406154042847.png?x-os)

案例5：查询 入职日期 在 '2000-01-01' (包含) 到 '2010-01-01'(包含) 之间 且 性别为女 的员工信息

~~~mysql
-- 方式1：
select id, username, password, name, gender, image, job, entrydate, create_time, update_time
from tb_emp
where entrydate>='2000-01-01' and entrydate<='2010-01-01'
      and gender = 2;;
-- 方式2： between...and
select id, username, password, name, gender, image, job, entrydate, create_time, update_time
from tb_emp
where entrydate between '2000-01-01' and '2010-01-01'
      and gender = 2;;
~~~

![image-20230406154216929](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406154216929.png?x-os)

案例6：查询 职位是 2 (讲师), 3 (学工主管), 4 (教研主管) 的员工信息

~~~mysql
-- 方式1：使用or连接多个条件
select id, username, password, name, gender, image, job, entrydate, create_time, update_time
from tb_emp
where job=2 or job=3 or job=4;
-- 方式2：in关键字
select id, username, password, name, gender, image, job, entrydate, create_time, update_time
from tb_emp
where job in (2,3,4);
~~~

![image-20230406154325114](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406154325114.png?x-os)



案例7：查询 姓名 为两个字的员工信息

~~~mysql
select id, username, password, name, gender, image, job, entrydate, create_time, update_time
from tb_emp
where name like '__';  # 通配符 "_" 代表任意1个字符
~~~

![image-20230406154510648](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406154510648.png?x-os)

案例8：查询 姓 '张' 的员工信息

~~~mysql
select id, username, password, name, gender, image, job, entrydate, create_time, update_time
from tb_emp
where name like '张%'; # 通配符 "%" 代表任意个字符（0个 ~ 多个）
~~~

![image-20230406154531058](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406154531058.png?x-os)

### 4.1.4  聚合函数

之前我们做的查询都是横向查询，就是根据条件一行一行的进行判断，而使用聚合函数查询就是纵向查询，它是对一列的值进行计算，然后返回一个结果值。（将一列数据作为一个整体，进行纵向计算）

语法：

~~~mysql
select  聚合函数(字段列表)  from  表名 ;
~~~

> 注意 : 聚合函数会忽略空值，对NULL值不作为统计。



常用聚合函数：

| **函数** | **功能** |
| -------- | -------- |
| count    | 统计数量 |
| max      | 最大值   |
| min      | 最小值   |
| avg      | 平均值   |
| sum      | 求和     |

> count ：按照列去统计有多少行数据。
>
> - 在根据指定的列统计的时候，如果这一列中有null的行，该行不会被统计在其中。
>
> sum ：计算指定列的数值和，如果不是数值类型，那么计算结果为0
>
> max ：计算指定列的最大值
>
> min ：计算指定列的最小值
>
> avg ：计算指定列的平均值



案例1：统计该企业员工数量

~~~mysql
# count(字段)
select count(id) from tb_emp;-- 结果：29
select count(job) from tb_emp;-- 结果：28 （聚合函数对NULL值不做计算）

# count(常量)
select count(0) from tb_emp;
select count('A') from tb_emp;

# count(*)  推荐此写法（MySQL底层进行了优化）
select count(*) from tb_emp;
~~~

![image-20230406155231424](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406155231424.png?x-os)

案例2：统计该企业最早入职的员工

~~~mysql
select * from tb_emp
where entrydate=(
    select  min(entrydate) from tb_emp
        );
~~~

![image-20230406155708585](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406155708585.png?x-os)



案例3：统计该企业员工 ID 的平均值

~~~mysql
select avg(id) from tb_emp;
~~~

![image-20230406155820517](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406155820517.png?x-os)

案例4：统计该企业员工的 ID 之和

~~~mysql
select sum(id) from tb_emp;
~~~

![image-20230406155851809](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406155851809.png?x-os)

### 4.1.5 分组查询

分组： 按照某一列或者某几列，把相同的数据进行合并输出。

> 分组其实就是按列进行分类(指定列下相同的数据归为一类)，然后可以对分类完的数据进行合并计算。
>
> 分组查询通常会使用聚合函数进行计算。

语法：

~~~mysql
select  字段列表  from  表名  [where 条件]  group by 分组字段名  [having 分组后过滤条件];
~~~



案例1：根据性别分组 , 统计男性和女性员工的数量

```mysql
select gender,count(*) from tb_emp
group by gender;
```

![image-20230406161257229](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406161257229.png?x-os)

案例2：查询入职时间在 '2015-01-01' (包含) 以前的员工 , 并对结果根据职位分组 , 获取员工数量大于等于2的职位

```mysql
select job,count(*) from tb_emp
where entrydate <= '2015-01-01'
group by job
having count(*)>=2;
```

![image-20230406161605957](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406161605957.png?x-os)





> 注意事项:
>
> ​	• 分组之后，查询的字段一般为聚合函数和分组字段，查询其他字段无任何意义
>
> ​	• 执行顺序：where > 聚合函数 > having 



**where与having区别（面试题）**

- 执行时机不同：where是分组之前进行过滤，不满足where条件，不参与分组；而having是分组之后对结果进行过滤。
- 判断条件不同：where不能对聚合函数进行判断，而having可以。

### 4.1.6 排序查询

排序在日常开发中是非常常见的一个操作，有升序排序，也有降序排序。

语法：

```mysql
select  字段列表  
from   表名   
[where  条件列表] 
[group by  分组字段 ] 
order  by  字段1  排序方式1 , 字段2  排序方式2 … ;
```

- 排序方式：

  - ASC ：升序（默认值）

  - DESC：降序



案例1：根据入职时间，对员工进行降序排序

```mysql
select * from tb_emp
order by entrydate desc;
```

![image-20230406201414323](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406201414323.png?x-os)

注意事项：如果是升序, 可以不指定排序方式ASC

案例2：根据入职时间对公司的员工进行升序排序，入职时间相同，再按照更新时间进行降序排序

```mysql
select * from tb_emp
order by entrydate,update_time desc;
```

![image-20230406201615576](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406201615576.png?x-os)

注意事项：如果是多字段排序，当第一个字段值相同时，才会根据第二个字段进行排序 

### 4.1.7 分页查询

分页操作在业务系统开发时，也是非常常见的一个功能，日常我们在网站中看到的各种各样的分页条，后台也都需要借助于数据库的分页操作。

分页查询语法：

```sql
select  字段列表  from   表名  limit  起始索引, 查询记录数 ;
```



案例1：从起始索引0开始查询员工数据, 每页展示5条记录

```mysql
select * from tb_emp
limit 5;
```

![image-20230406202514768](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406202514768.png?x-os)

案例2：查询 第3页 员工数据, 每页展示5条记录

```mysql
select * from tb_emp
limit 10,5;
```

![image-20230406202718910](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406202718910.png?x-os)



注意事项:

1. 起始索引从0开始。        计算公式 ：   起始索引 = （查询页码 - 1）* 每页显示记录数

2. 分页查询是数据库的方言，不同的数据库有不同的实现，MySQL中是LIMIT

3. 如果查询的是第一页数据，起始索引可以省略，直接简写为 limit  条数

### 4.1.8 案例

案例：根据需求完成员工信息的统计

> 分析：以上信息统计在开发中也叫图形报表(将统计好的数据以可视化的形式展示出来)
>
> - 员工性别统计：以饼状图的形式展示出企业男性员人数和女性员工人数
>   - 只要查询出男性员工和女性员工各自有多少人就可以了
> - 员工职位统计：以柱状图的形式展示各职位的在岗人数
>   - 只要查询出各个职位有多少人就可以了



- 员工性别统计：

```mysql
select if(gender=1,'男','女') 性别,count(*) 人数 from tb_emp
group by gender;
```

![image-20230406204137826](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406204137826.png?x-os)

if(表达式, tvalue, fvalue) ：当表达式为true时，取值tvalue；当表达式为false时，取值fvalue

- 员工职位统计：

```mysql
select case job
    when 1 then '班主任'
    when 2 then '讲师'
    when 3 then '学工主管'
    when 4 then '调研主管'
    else '灵活就业'
    end '工作',count(*) '人数'
    from tb_emp
group by job
```

![image-20230406204806884](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406204806884.png?x-os)

case   表达式    when   值1   then  结果1   [when 值2  then  结果2 ...]     [else result]     end

## 4.2 多表设计

关于单表的操作(单表的设计、单表的增删改查)我们就已经学习完了。接下来我们就要来学习多表的操作，首先来学习多表的设计。

项目开发中，在进行数据库表结构设计时，会根据业务需求及业务模块之间的关系，分析并设计表结构，由于业务之间相互关联，所以各个表结构之间也存在着各种联系，基本上分为三种：

- 一对多(多对一)

- 多对多

- 一对一

### 4.2.1 一对多

#### 4.2.1.1 外键约束

**问题分析**

在员工表的创建过程写入如下MySQL语句，为员工与部门表建立联系：

```mysql
dept_id     int unsigned comment '部门ID', -- 员工的归属部门
```

目前上述的两张表(员工表、部门表)，在数据库层面，并未建立关联，所以是无法保证数据的一致性和完整性的



**问题解决**

想解决上述的问题呢，我们就可以通过数据库中的 **外键约束** 来解决。

> 外键约束：让两张表的数据建立连接，保证数据的一致性和完整性。  
>
> 对应的关键字：foreign key

外键约束的语法：

```mysql
-- 创建表时指定
create table 表名(
	字段名    数据类型,
	...
	[constraint]   [外键名称]  foreign  key (外键字段名)   references   主表 (主表列名)	
);


-- 建完表后，添加外键
alter table  表名  add constraint  外键名称  foreign key(外键字段名) references 主表(主表列名);
```

那接下来，我们就为员工表的dept_id 建立外键约束，来关联部门表的主键。



方式1：通过SQL语句操作

```mysql
-- 修改表： 添加外键约束
alter table tb_emp  
add  constraint  fk_dept_id  foreign key (dept_id)  references  tb_dept(id);
```

方式2：图形化界面操作

> 当我们添加外键约束时，我们得保证当前数据库表中的数据是完整的。 

> 当我们添加了外键之后，再删除ID为1的部门，就会发现，无法删除。

![image-20230406211407530](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406211407530.png?x-os)

外键约束（foreign key）：保证了数据的完整性和一致性。

**物理外键和逻辑外键**

- 物理外键
  - 概念：使用foreign key定义外键关联另外一张表。
  - 缺点：
    - 影响增、删、改的效率（需要检查外键关系）。
    - 仅用于单节点数据库，不适用与分布式、集群场景。
    - 容易引发数据库的死锁问题，消耗性能。

- 逻辑外键
  - 概念：在业务层逻辑中，解决外键关联。
  - 通过逻辑外键，就可以很方便的解决上述问题。

> **在现在的企业开发中，很少会使用物理外键，都是使用逻辑外键。 甚至在一些数据库开发规范中，会明确指出禁止使用物理外键 foreign key **

### 4.2.2 一对一

一对一关系表在实际开发中应用起来比较简单，通常是用来做单表的拆分，也就是将一张大表拆分成两张小表，将大表中的一些基础字段放在一张表当中，将其他的字段放在另外一张表当中，以此来提高数据的操作效率。

> 一对一的应用场景： 用户表(基本信息+身份信息)
>
> ![image-20221207104508080](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20221207104508080.png?x-os)
>
> - 基本信息：用户的ID、姓名、性别、手机号、学历
> - 身份信息：民族、生日、身份证号、身份证签发机关，身份证的有效期(开始时间、结束时间)
>
> 如果在业务系统当中，对用户的基本信息查询频率特别的高，但是对于用户的身份信息查询频率很低，此时出于提高查询效率的考虑，我就可以将这张大表拆分成两张小表，第一张表存放的是用户的基本信息，而第二张表存放的就是用户的身份信息。他们两者之间一对一的关系，一个用户只能对应一个身份证，而一个身份证也只能关联一个用户。

那么在数据库层面怎么去体现上述两者之间是一对一的关系呢？

其实一对一我们可以看成一种特殊的一对多。一对多我们是怎么设计表关系的？是不是在多的一方添加外键。同样我们也可以通过外键来体现一对一之间的关系，我们只需要在任意一方来添加一个外键就可以了。

![image-20221207105632634](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20221207105632634.png?x-os)

> 一对一 ：在任意一方加入外键，关联另外一方的主键，并且设置外键为唯一的(UNIQUE)



### 4.2.3 多对多

多对多的关系在开发中属于也比较常见的。比如：学生和老师的关系，一个学生可以有多个授课老师，一个授课老师也可以有多个学生。在比如：学生和课程的关系，一个学生可以选修多门课程，一个课程也可以供多个学生选修。

案例：学生与课程的关系

- 关系：一个学生可以选修多门课程，一门课程也可以供多个学生选择

- 实现关系：建立第三张中间表，中间表至少包含两个外键，分别关联两方主键

![image-20221207113341028](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20221207113341028.png?x-os)

## 4.3 多表查询

### 4.3.1 介绍

多表查询：查询时从多张表中获取所需数据

> 单表查询的SQL语句：select  字段列表  from  表名;
>
> 那么要执行多表查询，只需要使用逗号分隔多张表即可，如： select   字段列表  from  表1, 表2;

查询用户表和部门表中的数据：

~~~mysql
select * from  tb_emp , tb_dept;
~~~

此时,我们看到查询结果中包含了大量的结果集，总共85条记录，而这其实就是员工表所有的记录(17行)与部门表所有记录(5行)的所有组合情况，这种现象称之为笛卡尔积。

笛卡尔积：笛卡尔乘积是指在数学中，两个集合(A集合和B集合)的所有组合情况。

在多表查询时，需要消除无效的笛卡尔积，只保留表关联部分的数据

在SQL语句中，如何去除无效的笛卡尔积呢？只需要给多表查询加上连接查询的条件即可。

~~~mysql
select * from tb_emp , tb_dept where tb_emp.dept_id = tb_dept.id ;
~~~

![image-20230406214435142](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406214435142.png?x-os)

由于id为17的员工，没有dept_id字段值，所以在多表查询时，根据连接查询的条件并没有查询到。

多表查询可以分为：

1. 内连接：相当于查询A、B交集部分数据

   ![image-20221207165446062](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20221207165446062.png?x-os) 

2. 外连接

   - 左外连接：查询左表所有数据(包括两张表交集部分数据)

   - 右外连接：查询右表所有数据(包括两张表交集部分数据)

3. 子查询

### 4.3.2 内连接

内连接查询：查询两表或多表中交集部分数据。

内连接从语法上可以分为：

- 隐式内连接

- 显式内连接

隐式内连接语法：

``` mysql
select  字段列表   from   表1 , 表2   where  条件 ... ;
```

显式内连接语法：

``` mysql
select  字段列表   from   表1  [ inner ]  join 表2  on  连接条件 ... ;
```



案例：查询员工的姓名及所属的部门名称

- 隐式内连接实现

  ```mysql
  select tb_emp.name 员工名, tb_dept.name 部门名
  from tb_emp,tb_dept
  where dept_id=tb_dept.id;
  ```

  ![image-20230406215427163](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406215427163.png?x-os)

- 显式内连接实现

```mysql
select tb_emp.name 员工名, tb_dept.name 部门名
from tb_emp inner join tb_dept  
    on dept_id = tb_dept.id;
```





多表查询时给表起别名：

- tableA  as  别名1  ,  tableB  as  别名2 ;

- tableA  别名1  ,  tableB  别名2 ;

使用了别名的多表查询：

~~~mysql
select emp.name , dept.name
from tb_emp emp inner join tb_dept dept
on emp.dept_id = dept.id;
~~~

> 注意事项:
>
> 一旦为表起了别名，就不能再使用表名来指定对应的字段了，此时只能够使用别名来指定字段。



### 4.3.3 外连接

外连接分为两种：左外连接 和 右外连接。

左外连接语法结构：

```mysql
select  字段列表   from   表1  left  [ outer ]  join 表2  on  连接条件 ... ;
```

> 左外连接相当于查询表1(左表)的所有数据，当然也包含表1和表2交集部分的数据。

右外连接语法结构：

```mysql
select  字段列表   from   表1  right  [ outer ]  join 表2  on  连接条件 ... ;
```

> 右外连接相当于查询表2(右表)的所有数据，当然也包含表1和表2交集部分的数据。



案例：查询员工表中所有员工的姓名, 和对应的部门名称

```mysql
select tb_emp.name 员工名, tb_dept.name 部门名
from tb_emp left join tb_dept
                   on dept_id = tb_dept.id;
```

![image-20230406220000579](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406220000579.png?x-os)

案例：查询部门表中所有部门的名称, 和对应的员工名称 

```mysql
select tb_dept.name 部门名, tb_emp.name 员工名
from tb_emp right join tb_dept
                   on dept_id = tb_dept.id;
```

![image-20230406220227586](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406220227586.png?x-os)

> 注意事项：
>
> 左外连接和右外连接是可以相互替换的，只需要调整连接查询时SQL语句中表的先后顺序就可以了。而我们在日常开发使用时，更偏向于左外连接。

### 4.3.4 子查询

#### 4.3.4.1 介绍

SQL语句中嵌套select语句，称为嵌套查询，又称子查询。

```sql
SELECT  *  FROM   t1   WHERE  column1 =  ( SELECT  column1  FROM  t2 ... );
```

> 子查询外部的语句可以是insert / update / delete / select 的任何一个，最常见的是 select。



根据子查询结果的不同分为：

1. 标量子查询（子查询结果为单个值[一行一列]）

2. 列子查询（子查询结果为一列，但可以是多行）

3. 行子查询（子查询结果为一行，但可以是多列）

4. 表子查询（子查询结果为多行多列[相当于子查询结果是一张表]）

子查询可以书写的位置：

1. where之后
2. from之后
3. select之后





#### 4.3.4.2 标量子查询

子查询返回的结果是单个值(数字、字符串、日期等)，最简单的形式，这种子查询称为标量子查询。

常用的操作符： =   <>   >    >=    <   <=   



案例1：查询"教研部"的所有员工信息

> 可以将需求分解为两步：
>
> 1. 查询 "教研部" 部门ID
> 2. 根据 "教研部" 部门ID，查询员工信息

```mysql
select * from tb_emp
where dept_id=
(select id from tb_dept
where name ='教研部');
```

![image-20230406221525570](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406221525570.png?x-os)

案例2：查询在 "方东白" 入职之后的员工信息

> 可以将需求分解为两步：
>
> 1. 查询 方东白 的入职日期
> 2. 查询 指定入职日期之后入职的员工信息

```mysql
select * from tb_emp
where entrydate >
(select entrydate from tb_emp
where name='方东白');
```

![image-20230406221721958](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406221721958.png?x-os)

#### 4.3.4.3 列子查询

子查询返回的结果是一列(可以是多行)，这种子查询称为列子查询。

常用的操作符：

| **操作符** | **描述**                     |
| ---------- | ---------------------------- |
| IN         | 在指定的集合范围之内，多选一 |
| NOT IN     | 不在指定的集合范围之内       |

案例：查询"教研部"和"咨询部"的所有员工信息

> 分解为以下两步：
>
> 1. 查询 "销售部" 和 "市场部" 的部门ID
> 2. 根据部门ID, 查询员工信息

```mysql
select * from tb_emp
where dept_id in
(select id from tb_dept
where name in('教研部','咨询部'));
```

![image-20230406222015225](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406222015225.png?x-os)

#### 4.3.4.4 行子查询

子查询返回的结果是一行(可以是多列)，这种子查询称为行子查询。

常用的操作符：= 、<> 、IN 、NOT IN



案例：查询与"韦一笑"的入职日期及职位都相同的员工信息 

> 可以拆解为两步进行： 
>
> 1. 查询 "韦一笑" 的入职日期 及 职位
> 2. 查询与"韦一笑"的入职日期及职位相同的员工信息 

```mysql
select * from tb_emp where (entrydate,job)=
(select entrydate,job from tb_emp
where name='韦一笑') and name!='韦一笑';
```

![image-20230406222942378](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406222942378.png?x-os)

#### 4.3.4.5 表子查询

子查询返回的结果是多行多列，常作为临时表，这种子查询称为表子查询。



案例：查询入职日期是 "2006-01-01" 之后的员工信息 , 及其部门信息

> 分解为两步执行：
>
> 1. 查询入职日期是 "2006-01-01" 之后的员工信息
> 2. 基于查询到的员工信息，在查询对应的部门信息

```mysql
select emp.* ,dept.name from
(select *  from tb_emp
where entrydate>'2006-01-01') emp left join tb_dept dept
on emp.dept_id=dept.id;
```

![image-20230406223910130](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230406223910130.png?x-os)

### 4.3.5 案例

基于之前设计的多表案例的表结构，我们来完成今天的多表查询案例需求。

**准备环境**

将资料中准备好的多表查询的数据准备的SQL脚本导入数据库中。

![image-20221208143318921](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20221208143318921.png?x-os) 

- 分类表：category
- 菜品表：dish
- 套餐表：setmeal
- 套餐菜品关系表：setmeal_dish

![image-20221208143312292](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20221208143312292.png?x-os) 



**需求实现**

-- 1. 查询价格低于 10元 的菜品的名称 、价格 及其 菜品的分类名称 .

```mysql
select dish.name,dish.price,category.name     
    from dish , category                    
where dish.category_id = category.id and price <10;
```

![image-20230407092807379](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230407092807379.png?x-os)

-- 2. 查询所有价格在 10元(含)到50元(含)之间 且 状态为'起售'的菜品名称、价格 及其 菜品的分类名称 (即使菜品没有分类 , 也需要将菜品查询出来).

```mysql
select d.name,d.price,c.name
from dish d left join category c on d.category_id = c.id
where d.price between 10 and 50
  and d.status = 1;
```

![image-20230407093949376](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230407093949376.png?x-os)

-- 3. 查询每个分类下最贵的菜品, 展示出分类的名称、最贵的菜品的价格 .

```mysql
select  c.name,max(price)
    from dish d,category c
where d.category_id=c.id
    group by c.name
```

![image-20230407094201936](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230407094201936.png?x-os)

-- 4. 查询各个分类下 状态为 '起售' , 并且 该分类下菜品总数量大于等于3 的 分类名称 .

```mysql
select c.name
    from dish d,category c
where d.category_id = c.id
and c.status=1
group by c.name
having count(*)>=3
```

![image-20230407095253019](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230407095253019.png?x-os)

-- 5. 查询出套餐中包含了哪些菜品 （展示出套餐名称、价格, 包含的菜品名称、价格、份数）.

```mysql
select s.name,s.price,d.name,d.price,sd.copies
    from setmeal s,dish d,setmeal_dish sd
where s.id = sd.setmeal_id and d.id = sd.dish_id;
```

![image-20230407095312081](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230407095312081.png?x-os)

-- 6. 查询出低于菜品平均价格的菜品信息 (展示出菜品名称、菜品价格).

```mysql
select d.name,d.price
    from dish d
where d.price <
(select avg(price)
from dish)
```

![image-20230407095731973](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230407095731973.png?x-os)

## 4.4 事务

场景：学工部整个部门解散了，该部门及部门下的员工都需要删除了。

- 操作：

  ```sql
  -- 删除学工部
  delete from dept where id = 1;  -- 删除成功
  
  -- 删除学工部的员工
  delete from emp where dept_id = 1; -- 删除失败（操作过程中出现错误：造成删除没有成功）
  ```

- 问题：如果删除部门成功了，而删除该部门的员工时失败了，此时就造成了数据的不一致。

​	要解决上述的问题，就需要通过数据库中的事务来解决。



### 4.4.1 介绍

在实际的业务开发中，有些业务操作要多次访问数据库。一个业务要发送多条SQL语句给数据库执行。需要将多次访问数据库的操作视为一个整体来执行，要么所有的SQL语句全部执行成功。如果其中有一条SQL语句失败，就进行事务的回滚，所有的SQL语句全部执行失败。

简而言之：事务是一组操作的集合，它是一个不可分割的工作单位。事务会把所有的操作作为一个整体一起向系统提交或撤销操作请求，即这些操作要么同时成功，要么同时失败。

事务作用：保证在一个事务中多次操作数据库表中数据时，要么全都成功,要么全都失败。



### 4.4.2 操作

MYSQL中有两种方式进行事务的操作：

1. 自动提交事务：即执行一条sql语句提交一次事务。（默认MySQL的事务是自动提交）
2. 手动提交事务：先开启，再提交 

事务操作有关的SQL语句：

| SQL语句                        | 描述             |
| ------------------------------ | ---------------- |
| start transaction;  /  begin ; | 开启手动控制事务 |
| commit;                        | 提交事务         |
| rollback;                      | 回滚事务         |

> 手动提交事务使用步骤：
>
> - 第1种情况：开启事务  =>  执行SQL语句   =>  成功  =>  提交事务
> - 第2种情况：开启事务  =>  执行SQL语句   =>  失败  =>  回滚事务



使用事务控制删除部门和删除该部门下的员工的操作：

```sql
-- 开启事务
start transaction ;

-- 删除学工部
delete from tb_dept where id = 1;

-- 删除学工部的员工
delete from tb_emp where dept_id = 1;
```

- 上述的这组SQL语句，如果如果执行成功，则提交事务

```sql
-- 提交事务 (成功时执行)
commit ;
```

- 上述的这组SQL语句，如果如果执行失败，则回滚事务

```sql
-- 回滚事务 (出错时执行)
rollback ;
```





### 4.4.3 四大特性

面试题：事务有哪些特性？

- 原子性（Atomicity）：事务是不可分割的最小单元，要么全部成功，要么全部失败。
- 一致性（Consistency）：事务完成时，必须使所有的数据都保持一致状态。
- 隔离性（Isolation）：数据库系统提供的隔离机制，保证事务在不受外部并发操作影响的独立环境下运行。
- 持久性（Durability）：事务一旦提交或回滚，它对数据库中的数据的改变就是永久的。

> 事务的四大特性简称为：ACID



- **原子性（Atomicity）** ：原子性是指事务包装的一组sql是一个不可分割的工作单元，事务中的操作要么全部成功，要么全部失败。

- **一致性（Consistency）**：一个事务完成之后数据都必须处于一致性状态。

​		如果事务成功的完成，那么数据库的所有变化将生效。

​		如果事务执行出现错误，那么数据库的所有变化将会被回滚(撤销)，返回到原始状态。

- **隔离性（Isolation）**：多个用户并发的访问数据库时，一个用户的事务不能被其他用户的事务干扰，多个并发的事务之间要相互隔离。

​		一个事务的成功或者失败对于其他的事务是没有影响。

- **持久性（Durability）**：一个事务一旦被提交或回滚，它对数据库的改变将是永久性的，哪怕数据库发生异常，重启之后数据亦然存在。



## 4.5 索引

### 4.5.1 介绍 

索引(index)：是帮助数据库高效获取数据的数据结构 。

- 简单来讲，就是使用索引可以提高查询的效率。



测试没有使用索引的查询：

![image-20221209115617429](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20221209115617429.png?x-os)

添加索引后查询：

~~~mysql
-- 添加索引
create index idx_sku_sn on tb_sku (sn);  #在添加索引时，也需要消耗时间

-- 查询数据（使用了索引）
select * from tb_sku where sn = '100000003145008';
~~~

![image-20221209120107543](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20221209120107543.png?x-os)



优点：

1. 提高数据查询的效率，降低数据库的IO成本。
2. 通过索引列对数据进行排序，降低数据排序的成本，降低CPU消耗。

缺点：

1. 索引会占用存储空间。
2. 索引大大提高了查询效率，同时却也降低了insert、update、delete的效率。





### 4.5.2 结构

MySQL数据库支持的索引结构有很多，如：Hash索引、B+Tree索引、Full-Text索引等。

我们平常所说的索引，如果没有特别指明，都是指默认的 B+Tree 结构组织的索引。

在没有了解B+Tree结构前，我们先回顾下之前所学习的树结构：

> 二叉查找树：左边的子节点比父节点小，右边的子节点比父节点大

![image-20221208174135229](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20221208174135229.png?x-os) 

> 当我们向二叉查找树保存数据时，是按照从大到小(或从小到大)的顺序保存的，此时就会形成一个单向链表，搜索性能会打折扣。

![image-20221208174859866](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20221208174859866.png?x-os) 

> 可以选择平衡二叉树或者是红黑树来解决上述问题。（红黑树也是一棵平衡的二叉树）

![image-20221209100647867](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20221209100647867.png?x-os)

> 但是在Mysql数据库中并没有使用二叉搜索数或二叉平衡数或红黑树来作为索引的结构。

思考：采用二叉搜索树或者是红黑树来作为索引的结构有什么问题？

<details>
    <summary>答案</summary>
    最大的问题就是在数据量大的情况下，树的层级比较深，会影响检索速度。因为不管是二叉搜索数还是红黑数，一个节点下面只能有两个子节点。此时在数据量大的情况下，就会造成数的高度比较高，树的高度一旦高了，检索速度就会降低。
</details>



> 说明：如果数据结构是红黑树，那么查询1000万条数据，根据计算树的高度大概是23左右，这样确实比之前的方式快了很多，但是如果高并发访问，那么一个用户有可能需要23次磁盘IO，那么100万用户，那么会造成效率极其低下。所以为了减少红黑树的高度，那么就得增加树的宽度，就是不再像红黑树一样每个节点只能保存一个数据，可以引入另外一种数据结构，一个节点可以保存多个数据，这样宽度就会增加从而降低树的高度。这种数据结构例如BTree就满足。

下面我们来看看B+Tree(多路平衡搜索树)结构中如何避免这个问题：

![image-20221208181315728](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20221208181315728.png?x-os)

B+Tree结构：

- 每一个节点，可以存储多个key（有n个key，就有n个指针）
- 节点分为：叶子节点、非叶子节点
  - 叶子节点，就是最后一层子节点，所有的数据都存储在叶子节点上
  - 非叶子节点，不是树结构最下面的节点，用于索引数据，存储的的是：key+指针
- 为了提高范围查询效率，叶子节点形成了一个双向链表，便于数据的排序及区间范围查询



> **拓展：**
>
> 非叶子节点都是由key+指针域组成的，一个key占8字节，一个指针占6字节，而一个节点总共容量是16KB，那么可以计算出一个节点可以存储的元素个数：16*1024字节 / (8+6)=1170个元素。
>
> - 查看mysql索引节点大小：show global status like 'innodb_page_size';    -- 节点大小：16384
>
> 当根节点中可以存储1170个元素，那么根据每个元素的地址值又会找到下面的子节点，每个子节点也会存储1170个元素，那么第二层即第二次IO的时候就会找到数据大概是：1170*1170=135W。也就是说B+Tree数据结构中只需要经历两次磁盘IO就可以找到135W条数据。
>
> 对于第二层每个元素有指针，那么会找到第三层，第三层由key+数据组成，假设key+数据总大小是1KB，而每个节点一共能存储16KB，所以一个第三层一个节点大概可以存储16个元素(即16条记录)。那么结合第二层每个元素通过指针域找到第三层的节点，第二层一共是135W个元素，那么第三层总元素大小就是：135W*16结果就是2000W+的元素个数。
>
> 结合上述分析B+Tree有如下优点：
>
> - 千万条数据，B+Tree可以控制在小于等于3的高度
> - 所有的数据都存储在叶子节点上，并且底层已经实现了按照索引进行排序，还可以支持范围查询，叶子节点是一个双向链表，支持从小到大或者从大到小查找





### 4.5.3 语法

**创建索引**

~~~mysql
create  [ unique ]  index 索引名 on  表名 (字段名,... ) ;
~~~

案例：为tb_emp表的name字段建立一个索引

~~~mysql
create index idx_emp_name on tb_emp(name);
~~~

> 在创建表时，如果添加了主键和唯一约束，就会默认创建：主键索引、唯一约束
>
> 



**查看索引**

~~~mysql
show  index  from  表名;
~~~

案例：查询 tb_emp 表的索引信息

~~~mysql
show  index  from  tb_emp;
~~~

![image-20230407103142096](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230407103142096.png?x-os)

**删除索引**

~~~mysql
drop  index  索引名  on  表名;
~~~

案例：删除 tb_emp 表中name字段的索引

~~~mysql
drop index idx_emp_name on tb_emp;
~~~



> 注意事项：
>
> - 主键字段，在建表时，会自动创建主键索引
>
> - 添加唯一约束时，数据库实际上会添加唯一索引



