# 1.概述

## 1.1 介绍

Apache Maven 是一个项目管理和构建工具，它基于项目对象模型（POM）的概念，通过一小段描述信息来管理项目的构建。

## 1.2 作用

![image-20230404141952945](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230404141952945.png?x-os)

- 依赖管理

![image-20230404142826829](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230404142826829.png?x-os)

- 统一项目结构

  ![image-20230404142517776](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230404142517776.png?x-os)

- 项目构建流程

  ![image-20230404142647697](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230404142647697.png?x-os)

## 1.3 仓库

- 仓库：用于存储资源，管理各种jar包
  - 本地仓库：自己计算机上的一个目录。
  - 中央仓库：由Maven团队维护的全球唯一的。
  - 远程仓库（私服）：一般由公司团队搭建的私有仓库。

![image-20230404144435990](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230404144435990.png?x-os)

## 1.4 安装

- 安装步骤

  - 解压apache-maven压缩包

  - 配置本地仓库：修改conf/settings.xml中的<localRespository>为一个指定目录。

    ```xml
    <localRepository>
    ...
    </localRepository>
    ```

  - 配置阿里云私服：修改conf/settings.xml中的<mirror>标签，为其添加如下子标签：

    ```xml
    <mirror>  
        <id>alimaven</id>  
        <name>aliyun maven</name>   <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
        <mirrorOf>central</mirrorOf>          
    </mirror>
    ```

  - 配置环境变量：MAVEN_HOME为maven的解压目录，并将其bin目录加入PATH环境变量。

  - 测试：

    ```bash
    mvn -v
    ```

## 1.5 IDEA集成

### 1.5.1 配置Maven环境

- 选择IDEA中File-->Settings-->Build,Execution,Development-->Build Tools-->Maven
- 设置IDEA使用本地安装的Maven，并修改文件及本地仓库路径

### 1.5.2 Maven坐标

- 什么是坐标？

  * Maven中的坐标是资源的唯一标识 , 通过该坐标可以唯一定位资源位置

  * 使用坐标来定义项目或引入项目中需要的依赖


- Maven坐标主要组成

  * groupId：定义当前Maven项目隶属组织名称（通常是域名反写，例如：com.itheima）

  * artifactId：定义当前Maven项目名称（通常是模块名称，例如 order-service、goods-service）

  * version：定义当前项目版本号

```xml
<groupId>com.itheima</groupId>
<artifactId>maven-project01</artifactId>
<version>1.0-SNAPSHOT</version>
```

### 1.5.3 导入Maven项目

- 打开IDEA，选择右侧Maven面板，点击 + 号，选中对应项目的pom.xml文件，双击即可

![image-20230404153745701](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230404153745701.png?x-os)

## 1.6 依赖管理

### 1.6.1 依赖配置

- 依赖：指当前项目运行所需要的jar包。一个项目中可以引入多个依赖：


- 例如：在当前工程中，我们需要用到logback来记录日志，此时就可以在maven工程的pom.xml文件中，引入logback的依赖。具体步骤如下：

  - 在pom.xml中编写<dependencies>标签


  - 在<dependencies>标签中使用<dependency>引入坐标


  - 定义坐标的 groupId、artifactId、version
  - 点击刷新按钮，引入最新加入的坐标

![image-20230404154639682](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230404154639682.png?x-os)

### 1.6.2 依赖传递

- 依赖传递可以分为：

  - 直接依赖：在当前项目中通过依赖配置建立的依赖关系


  - 间接依赖：被依赖的资源如果依赖其他资源，当前项目间接依赖其他资源

![image-20230404155138194](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230404155138194.png?x-os)

![image-20230404160429527](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230404160429527.png?x-os)

- 排除依赖：指主动断开依赖的资源。（被排除的资源无需指定版本)

  ```xml
          <dependency>
              <groupId>com.itheima</groupId>
              <artifactId>maven-projectB</artifactId>
              <version>1.0-SNAPSHOT</version>
              <!-- 排除依赖-->
              <exclusions>
                  <exclusion>
                      <groupId>junit</groupId>
                      <artifactId>junit</artifactId>
                  </exclusion>
              </exclusions>
          </dependency>
  ```

  ![image-20230404160517694](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230404160517694.png?x-os)

### 1.6.3 依赖范围

在项目中导入依赖的jar包后，默认情况下，可以在任何地方使用。

如果希望限制依赖的使用范围，可以通过<scope>标签设置其作用范围。

作用范围：

1. 主程序范围有效（main文件夹范围内）

2. 测试程序范围有效（test文件夹范围内）

3. 是否参与打包运行（package指令范围内）

![image-20230404160833309](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230404160833309.png?x-os)

![image-20230404160811774](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230404160811774.png?x-os)

如指定只能在test环境使用logback：

```xml
    <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-classic</artifactId>
            <version>1.2.3</version>
            <scope>test</scope>
        </dependency>
```



### 1.6.4 生命周期

Maven的生命周期就是为了对所有的构建过程进行抽象和统一。

Maven对项目构建的生命周期划分为3套（相互独立）：

- clean：清理工作。

- default：核心工作。如：编译、测试、打包、安装、部署等。

- site：生成报告、发布站点等。

  ![image-20230404162258926](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230404162258926.png?x-os)

  常见的生命周期如下所示：

![image-20230404161925452](https://typora-picsbed.oss-cn-shanghai.aliyuncs.com/typora/image-20230404161925452.png?x-os)

- 在同一套生命周期中，我们在执行后面的生命周期时，前面的生命周期都会执行。由于clean和其他常见生命周期属于不同套生命周期，故install指令执行不会执行clean指令，但会依次执行同一套default生命周期中的compile、test、package指令

在日常开发中，当我们要执行指定的生命周期时，有两种执行方式：

1. 在idea工具右侧的maven工具栏中，选择对应的生命周期，双击执行
2. 在DOS命令行中，通过maven命令执行，如

   ```bash
   mvn clean
   ```

   