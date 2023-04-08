# 1. 登录功能

## 1.1 需求

![image-20230105085404855](https://typora-picsbed-new.oss-cn-shanghai.aliyuncs.com/typora/image-20230105085404855.png?x-os)

在登录界面中，我们可以输入用户的用户名以及密码，然后点击 "登录" 按钮就要请求服务器，服务端判断用户输入的用户名或者密码是否正确。如果正确，则返回成功结果，前端跳转至系统首页面。



## 1.2 接口文档

我们参照接口文档来开发登录功能

- 基本信息

  ~~~
  请求路径：/login
  
  请求方式：POST
  
  接口描述：该接口用于员工登录Tlias智能学习辅助系统，登录完毕后，系统下发JWT令牌。 
  ~~~

- 请求参数

  参数格式：application/json

  参数说明：

  | 名称     | 类型   | 是否必须 | 备注   |
  | -------- | ------ | -------- | ------ |
  | username | string | 必须     | 用户名 |
  | password | string | 必须     | 密码   |

  请求数据样例：

  ~~~json
  {
  	"username": "jinyong",
      "password": "123456"
  }
  ~~~

- 响应数据

  参数格式：application/json

  参数说明：

  | 名称 | 类型   | 是否必须 | 默认值 | 备注                     | 其他信息 |
  | ---- | ------ | -------- | ------ | ------------------------ | -------- |
  | code | number | 必须     |        | 响应码, 1 成功 ; 0  失败 |          |
  | msg  | string | 非必须   |        | 提示信息                 |          |
  | data | string | 必须     |        | 返回的数据 , jwt令牌     |          |

  响应数据样例：

  ~~~json
  {
    "code": 1,
    "msg": "success",
    "data": "eyJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoi6YeR5bq4IiwiaWQiOjEsInVzZXJuYW1lIjoiamlueW9uZyIsImV4cCI6MTY2MjIwNzA0OH0.KkUc_CXJZJ8Dd063eImx4H9Ojfrr6XMJ-yVzaWCVZCo"
  }
  ~~~



## 1.3 思路分析

![image-20230105175310401](https://typora-picsbed-new.oss-cn-shanghai.aliyuncs.com/typora/image-20230105175310401.png?x-os)

登录服务端的核心逻辑就是：接收前端请求传递的用户名和密码 ，然后再根据用户名和密码查询用户信息，如果用户信息存在，则说明用户输入的用户名和密码正确。如果查询到的用户不存在，则说明用户输入的用户名和密码错误。



## 1.4 功能开发

**LoginController**

~~~java
@RestController
public class LoginController {

    @Autowired
    private EmpService empService;

    @PostMapping("/login")
    public Result login(@RequestBody Emp emp){
        Emp e = empService.login(emp);
	    return  e != null ? Result.success():Result.error("用户名或密码错误");
    }
}
~~~

**EmpService**

~~~java
public interface EmpService {

    /**
     * 用户登录
     * @param emp
     * @return
     */
    public Emp login(Emp emp);

    //省略其他代码...
}
~~~

**EmpServiceImpl**

~~~java
@Slf4j
@Service
public class EmpServiceImpl implements EmpService {
    @Autowired
    private EmpMapper empMapper;

    @Override
    public Emp login(Emp emp) {
        //调用dao层功能：登录
        Emp loginEmp = empMapper.getByUsernameAndPassword(emp);

        //返回查询结果给Controller
        return loginEmp;
    }   
    
    //省略其他代码...
}
~~~

**EmpMapper**

~~~java
@Mapper
public interface EmpMapper {

    @Select("select id, username, password, name, gender, image, job, entrydate, dept_id, create_time, update_time " +
            "from emp " +
            "where username=#{username} and password =#{password}")
    public Emp getByUsernameAndPassword(Emp emp);
    
    //省略其他代码...
}
~~~



## 1.5 测试

功能开发完毕后，我们就可以启动服务，打开postman进行测试了。 

发起POST请求，访问：http://localhost:8080/login

![image-20230408222039006](https://typora-picsbed-new.oss-cn-shanghai.aliyuncs.com/typora/image-20230408222039006.png?x-os)

# 2. 登录校验

## 2.1 问题分析

我们已经完成了基础登录功能的开发与测试，在我们登录成功后就可以进入到后台管理系统中进行数据的操作。

但是当我们在浏览器中新的页面上输入地址：`http://localhost:9528/#/system/dept`，发现没有登录仍然可以进入到后端管理系统页面。

而真正的登录功能应该是：登陆后才能访问后端系统页面，不登陆则跳转登陆页面进行登陆。

为什么会出现这个问题？其实原因很简单，就是因为针对于我们当前所开发的部门管理、员工管理以及文件上传等相关接口来说，我们在服务器端并没有做任何的判断，没有去判断用户是否登录了。所以无论用户是否登录，都可以访问部门管理以及员工管理的相关数据。所以我们目前所开发的登录功能，它只是徒有其表。而我们要想解决这个问题，我们就需要完成一步非常重要的操作：登录校验。

![image-20230105180811717](https://typora-picsbed-new.oss-cn-shanghai.aliyuncs.com/typora/image-20230105180811717.png?x-os)



什么是登录校验？

- 所谓登录校验，指的是我们在服务器端接收到浏览器发送过来的请求之后，首先我们要对请求进行校验。先要校验一下用户登录了没有，如果用户已经登录了，就直接执行对应的业务操作就可以了；如果用户没有登录，此时就不允许他执行相关的业务操作，直接给前端响应一个错误的结果，最终跳转到登录页面，要求他登录成功之后，再来访问对应的数据。



了解完什么是登录校验之后，接下来我们分析一下登录校验大概的实现思路。



首先我们在宏观上先有一个认知：

前面在讲解HTTP协议的时候，我们提到HTTP协议是无状态协议。什么又是无状态的协议？

所谓无状态，指的是每一次请求都是独立的，下一次请求并不会携带上一次请求的数据。而浏览器与服务器之间进行交互，基于HTTP协议也就意味着现在我们通过浏览器来访问了登陆这个接口，实现了登陆的操作，接下来我们在执行其他业务操作时，服务器也并不知道这个员工到底登陆了没有。因为HTTP协议是无状态的，两次请求之间是独立的，所以是无法判断这个员工到底登陆了没有。



![image-20230105194710533](https://typora-picsbed-new.oss-cn-shanghai.aliyuncs.com/typora/image-20230105194710533.png?x-os)

那应该怎么来实现登录校验的操作呢？具体的实现思路可以分为两部分：

1. 在员工登录成功后，需要将用户登录成功的信息存起来，记录用户已经登录成功的标记。
2. 在浏览器发起请求时，需要在服务端进行统一拦截，拦截后进行登录校验。

> 想要判断员工是否已经登录，我们需要在员工登录成功之后，存储一个登录成功的标记，接下来在每一个接口方法执行之前，先做一个条件判断，判断一下这个员工到底登录了没有。如果是登录了，就可以执行正常的业务操作，如果没有登录，会直接给前端返回一个错误的信息，前端拿到这个错误信息之后会自动的跳转到登录页面。
>
> 我们程序中所开发的查询功能、删除功能、添加功能、修改功能，都需要使用以上套路进行登录校验。此时就会出现：相同代码逻辑，每个功能都需要编写，就会造成代码非常繁琐。
>
> 为了简化这块操作，我们可以使用一种技术：统一拦截技术。
>
> 通过统一拦截的技术，我们可以来拦截浏览器发送过来的所有的请求，拦截到这个请求之后，就可以通过请求来获取之前所存入的登录标记，在获取到登录标记且标记为登录成功，就说明员工已经登录了。如果已经登录，我们就直接放行(意思就是可以访问正常的业务接口了)。



我们要完成以上操作，会涉及到web开发中的两个技术：

1. 会话技术
2. 统一拦截技术



而统一拦截技术现实方案也有两种：

1. Servlet规范中的Filter过滤器
2. Spring提供的interceptor拦截器

下面我们先学习会话技术，然后再学习统一拦截技术。







## 2.2 会话技术

介绍了登录校验的大概思路之后，我们先来学习下会话技术。

### 2.2.1 会话技术介绍

什么是会话？

- 在我们日常生活当中，会话指的就是谈话、交谈。

- 在web开发当中，会话指的就是浏览器与服务器之间的一次连接，我们就称为一次会话。

  > 在用户打开浏览器第一次访问服务器的时候，这个会话就建立了，直到有任何一方断开连接，此时会话就结束了。在一次会话当中，是可以包含多次请求和响应的。
  >
  > 比如：打开了浏览器来访问web服务器上的资源（浏览器不能关闭、服务器不能断开）
  >
  > - 第1次：访问的是登录的接口，完成登录操作
  > - 第2次：访问的是部门管理接口，查询所有部门数据
  > - 第3次：访问的是员工管理接口，查询员工数据
  >
  > 只要浏览器和服务器都没有关闭，以上3次请求都属于一次会话当中完成的。

![image-20230105203827355](https://typora-picsbed-new.oss-cn-shanghai.aliyuncs.com/typora/image-20230105203827355.png?x-os)

需要注意的是：会话是和浏览器关联的，当有三个浏览器客户端和服务器建立了连接时，就会有三个会话。同一个浏览器在未关闭之前请求了多次服务器，这多次请求是属于同一个会话。比如：1、2、3这三个请求都是属于同一个会话。当我们关闭浏览器之后，这次会话就结束了。而如果我们是直接把web服务器关了，那么所有的会话就都结束了。



知道了会话的概念了，接下来我们再来了解下会话跟踪。

会话跟踪：一种维护浏览器状态的方法，服务器需要识别多次请求是否来自于同一浏览器，以便在同一次会话的多次请求间共享数据。

> 服务器会接收很多的请求，但是服务器是需要识别出这些请求是不是同一个浏览器发出来的。比如：1和2这两个请求是不是同一个浏览器发出来的，3和5这两个请求不是同一个浏览器发出来的。如果是同一个浏览器发出来的，就说明是同一个会话。如果是不同的浏览器发出来的，就说明是不同的会话。而识别多次请求是否来自于同一浏览器的过程，我们就称为会话跟踪。

我们使用会话跟踪技术就是要完成在同一个会话中，多个请求之间进行共享数据。

> 为什么要共享数据呢？
>
> 由于HTTP是无状态协议，在后面请求中怎么拿到前一次请求生成的数据呢？此时就需要在一次会话的多次请求之间进行数据共享



会话跟踪技术有两种：

1. Cookie（客户端会话跟踪技术）
   - 数据存储在客户端浏览器当中
2. Session（服务端会话跟踪技术）
   - 数据存储在储在服务端
3. 令牌技术





### 2.2.2 会话跟踪方案

上面我们介绍了什么是会话，什么是会话跟踪，并且也提到了会话跟踪 3 种常见的技术方案。接下来，我们就来对比一下这 3 种会话跟踪的技术方案，来看一下具体的实现思路，以及它们之间的优缺点。

#### 2.2.2.1 方案一 - Cookie

cookie 是客户端会话跟踪技术，它是存储在客户端浏览器的，我们使用 cookie 来跟踪会话，我们就可以在浏览器第一次发起请求来请求服务器的时候，我们在服务器端来设置一个cookie。

比如第一次请求了登录接口，登录接口执行完成之后，我们就可以设置一个cookie，在 cookie 当中我们就可以来存储用户相关的一些数据信息。比如我可以在 cookie 当中来存储当前登录用户的用户名，用户的ID。

服务器端在给客户端在响应数据的时候，会**自动**的将 cookie 响应给浏览器，浏览器接收到响应回来的 cookie 之后，会**自动**的将 cookie 的值存储在浏览器本地。接下来在后续的每一次请求当中，都会将浏览器本地所存储的 cookie **自动**地携带到服务端。

![image-20230112101901417](https://typora-picsbed-new.oss-cn-shanghai.aliyuncs.com/typora/image-20230112101901417.png?x-os) 

接下来在服务端我们就可以获取到 cookie 的值。我们可以去判断一下这个 cookie 的值是否存在，如果不存在这个cookie，就说明客户端之前是没有访问登录接口的；如果存在 cookie 的值，就说明客户端之前已经登录完成了。这样我们就可以基于 cookie 在同一次会话的不同请求之间来共享数据。



我刚才在介绍流程的时候，用了 3 个自动：

- 服务器会 **自动** 的将 cookie 响应给浏览器。

- 浏览器接收到响应回来的数据之后，会 **自动** 的将 cookie 存储在浏览器本地。

- 在后续的请求当中，浏览器会 **自动** 的将 cookie 携带到服务器端。

  

**为什么这一切都是自动化进行的？**

是因为 cookie 它是 HTP 协议当中所支持的技术，而各大浏览器厂商都支持了这一标准。在 HTTP 协议官方给我们提供了一个响应头和请求头：

- 响应头 Set-Cookie ：设置Cookie数据的

- 请求头 Cookie：携带Cookie数据的

![image-20230112101804878](https://typora-picsbed-new.oss-cn-shanghai.aliyuncs.com/typora/image-20230112101804878.png?x-os) 



**代码测试**

```java
@Slf4j
@RestController
public class SessionController {

    //设置Cookie
    @GetMapping("/c1")
    public Result cookie1(HttpServletResponse response){
        response.addCookie(new Cookie("login_username","itheima")); //设置Cookie/响应Cookie
        return Result.success();
    }
	
    //获取Cookie
    @GetMapping("/c2")
    public Result cookie2(HttpServletRequest request){
        Cookie[] cookies = request.getCookies();
        for (Cookie cookie : cookies) {
            if(cookie.getName().equals("login_username")){
                System.out.println("login_username: "+cookie.getValue()); //输出name为login_username的cookie
            }
        }
        return Result.success();
    }
}    
```



A. 访问c1接口，设置Cookie，http://localhost:8080/c1

![image-20230112105410076](https://typora-picsbed-new.oss-cn-shanghai.aliyuncs.com/typora/image-20230112105410076.png?x-os) 

我们可以看到，设置的cookie，通过**响应头Set-Cookie**响应给浏览器，并且浏览器会将Cookie，存储在浏览器端。

![image-20230112105538131](https://typora-picsbed-new.oss-cn-shanghai.aliyuncs.com/typora/image-20230112105538131.png?x-os) 



B. 访问c2接口 http://localhost:8080/c2，此时浏览器会自动的将Cookie携带到服务端，是通过**请求头Cookie**，携带的。

![image-20230112105658486](https://typora-picsbed-new.oss-cn-shanghai.aliyuncs.com/typora/image-20230112105658486.png?x-os) 





**优缺点**

- 优点：HTTP协议中支持的技术（像Set-Cookie 响应头的解析以及 Cookie 请求头数据的携带，都是浏览器自动进行的，是无需我们手动操作的）
- 缺点：
  - 移动端APP(Android、IOS)中无法使用Cookie
  - 不安全，用户可以自己禁用Cookie
  - Cookie不能跨域

> 跨域介绍：
>
> ​	 <img src="https://typora-picsbed-new.oss-cn-shanghai.aliyuncs.com/typora/image-20230112103840467.png?x-os" alt="image-20230112103840467" style="zoom:80%;" /> 
>
> - 现在的项目，大部分都是前后端分离的，前后端最终也会分开部署，前端部署在服务器 192.168.150.200 上，端口 80，后端部署在 192.168.150.100上，端口 8080
> - 我们打开浏览器直接访问前端工程，访问url：http://192.168.150.200/login.html
> - 然后在该页面发起请求到服务端，而服务端所在地址不再是localhost，而是服务器的IP地址192.168.150.100，假设访问接口地址为：http://192.168.150.100:8080/login
> - 那此时就存在跨域操作了，因为我们是在 http://192.168.150.200/login.html 这个页面上访问了http://192.168.150.100:8080/login 接口
> - 此时如果服务器设置了一个Cookie，这个Cookie是不能使用的，因为Cookie无法跨域
>
> 
>
> 区分跨域的维度：
>
> - 协议
> - IP/协议
> - 端口
>
> 只要上述的三个维度有任何一个维度不同，那就是跨域操作
>
> 
>
> 举例：
>
> ​	http://192.168.150.200/login.html ----------> https://192.168.150.200/login   		[协议不同，跨域]
>
> ​	http://192.168.150.200/login.html ----------> http://192.168.150.100/login     		[IP不同，跨域]
>
> ​	http://192.168.150.200/login.html ----------> http://192.168.150.200:8080/login   [端口不同，跨域]
>
> ​    http://192.168.150.200/login.html ----------> http://192.168.150.200/login    		 [不跨域]   



#### 2.2.2.2 方案二 - Session

前面介绍的时候，我们提到Session，它是服务器端会话跟踪技术，所以它是存储在服务器端的。而 Session 的底层其实就是基于我们刚才所介绍的 Cookie 来实现的。

 

- 获取Session

  ![image-20230112105938545](https://typora-picsbed-new.oss-cn-shanghai.aliyuncs.com/typora/image-20230112105938545.png?x-os) 

  如果我们现在要基于 Session 来进行会话跟踪，浏览器在第一次请求服务器的时候，我们就可以直接在服务器当中来获取到会话对象Session。如果是第一次请求Session ，会话对象是不存在的，这个时候服务器会自动的创建一个会话对象Session 。而每一个会话对象Session ，它都有一个ID（示意图中Session后面括号中的1，就表示ID），我们称之为 Session 的ID。

 

- 响应Cookie (JSESSIONID)

  ![image-20230112110441075](https://typora-picsbed-new.oss-cn-shanghai.aliyuncs.com/typora/image-20230112110441075.png?x-os) 

  接下来，服务器端在给浏览器响应数据的时候，它会将 Session 的 ID 通过 Cookie 响应给浏览器。其实在响应头当中增加了一个 Set-Cookie 响应头。这个  Set-Cookie  响应头对应的值是不是cookie？ cookie 的名字是固定的 JSESSIONID 代表的服务器端会话对象 Session 的 ID。浏览器会自动识别这个响应头，然后自动将Cookie存储在浏览器本地。



- 查找Session

  ![image-20230112101943835](https://typora-picsbed-new.oss-cn-shanghai.aliyuncs.com/typora/image-20230112101943835.png?x-os) 

  接下来，在后续的每一次请求当中，都会将 Cookie 的数据获取出来，并且携带到服务端。接下来服务器拿到JSESSIONID这个 Cookie 的值，也就是 Session 的ID。拿到 ID 之后，就会从众多的 Session 当中来找到当前请求对应的会话对象Session。

  

  这样我们是不是就可以通过 Session 会话对象在同一次会话的多次请求之间来共享数据了？好，这就是基于 Session 进行会话跟踪的流程。



**代码测试**

```java
@Slf4j
@RestController
public class SessionController {

    @GetMapping("/s1")
    public Result session1(HttpSession session){
        log.info("HttpSession-s1: {}", session.hashCode());

        session.setAttribute("loginUser", "tom"); //往session中存储数据
        return Result.success();
    }

    @GetMapping("/s2")
    public Result session2(HttpServletRequest request){
        HttpSession session = request.getSession();
        log.info("HttpSession-s2: {}", session.hashCode());

        Object loginUser = session.getAttribute("loginUser"); //从session中获取数据
        log.info("loginUser: {}", loginUser);
        return Result.success(loginUser);
    }
}
```



A. 访问 s1 接口，http://localhost:8080/s1

![image-20230112111004447](https://typora-picsbed-new.oss-cn-shanghai.aliyuncs.com/typora/image-20230112111004447.png?x-os) 

请求完成之后，在响应头中，就会看到有一个Set-Cookie的响应头，里面响应回来了一个Cookie，就是JSESSIONID，这个就是服务端会话对象 Session 的ID。



B. 访问 s2 接口，http://localhost:8080/s2

![image-20230112111137207](https://typora-picsbed-new.oss-cn-shanghai.aliyuncs.com/typora/image-20230112111137207.png?x-os) 

接下来，在后续的每次请求时，都会将Cookie的值，携带到服务端，那服务端呢，接收到Cookie之后，会自动的根据JSESSIONID的值，找到对应的会话对象Session。



那经过这两步测试，大家也会看到，在控制台中输出如下日志：

![image-20230112111328117](https://typora-picsbed-new.oss-cn-shanghai.aliyuncs.com/typora/image-20230112111328117.png?x-os) 

两次请求，获取到的Session会话对象的hashcode是一样的，就说明是同一个会话对象。而且，第一次请求时，往Session会话对象中存储的值，第二次请求时，也获取到了。 那这样，我们就可以通过Session会话对象，在同一个会话的多次请求之间来进行数据共享了。



**优缺点**

- 优点：Session是存储在服务端的，安全
- 缺点：
  - 服务器集群环境下无法直接使用Session
  - 移动端APP(Android、IOS)中无法使用Cookie
  - 用户可以自己禁用Cookie
  - Cookie不能跨域

> PS：Session 底层是基于Cookie实现的会话跟踪，如果Cookie不可用，则该方案，也就失效了。



> 服务器集群环境为何无法使用Session？
>
> ​	<img src="https://typora-picsbed-new.oss-cn-shanghai.aliyuncs.com/typora/image-20230112112557480.png?x-os" alt="image-20230112112557480" style="zoom:67%;" /> 
>
> - 首先第一点，我们现在所开发的项目，一般都不会只部署在一台服务器上，因为一台服务器会存在一个很大的问题，就是单点故障。所谓单点故障，指的就是一旦这台服务器挂了，整个应用都没法访问了。
>
> ​    ![image-20230112112740131](https://typora-picsbed-new.oss-cn-shanghai.aliyuncs.com/typora/image-20230112112740131.png?x-os) 
>
> - 所以在现在的企业项目开发当中，最终部署的时候都是以集群的形式来进行部署，也就是同一个项目它会部署多份。比如这个项目我们现在就部署了 3 份。
>
> - 而用户在访问的时候，到底访问这三台其中的哪一台？其实用户在访问的时候，他会访问一台前置的服务器，我们叫负载均衡服务器，我们在后面项目当中会详细讲解。目前大家先有一个印象负载均衡服务器，它的作用就是将前端发起的请求均匀的分发给后面的这三台服务器。
>
>   ![image-20230112113558810](https://typora-picsbed-new.oss-cn-shanghai.aliyuncs.com/typora/image-20230112113558810.png?x-os) 
>
> - 此时假如我们通过 session 来进行会话跟踪，可能就会存在这样一个问题。用户打开浏览器要进行登录操作，此时会发起登录请求。登录请求到达负载均衡服务器，将这个请求转给了第一台 Tomcat 服务器。
>
>   Tomcat 服务器接收到请求之后，要获取到会话对象session。获取到会话对象 session 之后，要给浏览器响应数据，最终在给浏览器响应数据的时候，就会携带这么一个 cookie 的名字，就是 JSESSIONID ，下一次再请求的时候，是不是又会将 Cookie 携带到服务端？
>
>   好。此时假如又执行了一次查询操作，要查询部门的数据。这次请求到达负载均衡服务器之后，负载均衡服务器将这次请求转给了第二台 Tomcat 服务器，此时他就要到第二台 Tomcat 服务器当中。根据JSESSIONID 也就是对应的 session 的 ID 值，要找对应的 session 会话对象。
>
>   我想请问在第二台服务器当中有没有这个ID的会话对象 Session， 是没有的。此时是不是就出现问题了？我同一个浏览器发起了 2 次请求，结果获取到的不是同一个会话对象，这就是Session这种会话跟踪方案它的缺点，在服务器集群环境下无法直接使用Session。





大家会看到上面这两种传统的会话技术，在现在的企业开发当中是不是会存在很多的问题。 为了解决这些问题，在现在的企业开发当中，基本上都会采用第三种方案，通过令牌技术来进行会话跟踪。接下来我们就来介绍一下令牌技术，来看一下令牌技术又是如何跟踪会话的。



#### 2.2.2.3 方案三 - 令牌技术

这里我们所提到的令牌，其实它就是一个用户身份的标识，看似很高大上，很神秘，其实本质就是一个字符串。

![image-20230112102022634](https://typora-picsbed-new.oss-cn-shanghai.aliyuncs.com/typora/image-20230112102022634.png?x-os) 

如果通过令牌技术来跟踪会话，我们就可以在浏览器发起请求。在请求登录接口的时候，如果登录成功，我就可以生成一个令牌，令牌就是用户的合法身份凭证。接下来我在响应数据的时候，我就可以直接将令牌响应给前端。

接下来我们在前端程序当中接收到令牌之后，就需要将这个令牌存储起来。这个存储可以存储在 cookie 当中，也可以存储在其他的存储空间(比如：localStorage)当中。

接下来，在后续的每一次请求当中，都需要将令牌携带到服务端。携带到服务端之后，接下来我们就需要来校验令牌的有效性。如果令牌是有效的，就说明用户已经执行了登录操作，如果令牌是无效的，就说明用户之前并未执行登录操作。

此时，如果是在同一次会话的多次请求之间，我们想共享数据，我们就可以将共享的数据存储在令牌当中就可以了。



**优缺点**

- 优点：
  - 支持PC端、移动端
  - 解决集群环境下的认证问题
  - 减轻服务器的存储压力（无需在服务器端存储）
- 缺点：需要自己实现（包括令牌的生成、令牌的传递、令牌的校验）





**针对于这三种方案，现在企业开发当中使用的最多的就是第三种令牌技术进行会话跟踪。而前面的这两种传统的方案，现在企业项目开发当中已经很少使用了。所以在我们的课程当中，我们也将会采用令牌技术来解决案例项目当中的会话跟踪问题。**





## 2.3 JWT令牌

前面我们介绍了基于令牌技术来实现会话追踪。这里所提到的令牌就是用户身份的标识，其本质就是一个字符串。令牌的形式有很多，我们使用的是功能强大的 JWT令牌。

### 2.3.1 介绍

JWT全称：JSON Web Token  （官网：https://jwt.io/）

- 定义了一种简洁的、自包含的格式，用于在通信双方以json数据格式安全的传输信息。由于数字签名的存在，这些信息是可靠的。

  > 简洁：是指jwt就是一个简单的字符串。可以在请求参数或者是请求头当中直接传递。
  >
  > 自包含：指的是jwt令牌，看似是一个随机的字符串，但是我们是可以根据自身的需求在jwt令牌中存储自定义的数据内容。如：可以直接在jwt令牌中存储用户的相关信息。
  >
  > 简单来讲，jwt就是将原始的json数据格式进行了安全的封装，这样就可以直接基于jwt在通信双方安全的进行信息传输了。



JWT的组成： （JWT令牌由三个部分组成，三个部分之间使用英文的点来分割）

- 第一部分：Header(头）， 记录令牌类型、签名算法等。 例如：{"alg":"HS256","type":"JWT"}

- 第二部分：Payload(有效载荷），携带一些自定义信息、默认信息等。 例如：{"id":"1","username":"Tom"}

- 第三部分：Signature(签名），防止Token被篡改、确保安全性。将header、payload，并加入指定秘钥，通过指定签名算法计算而来。

  >签名的目的就是为了防jwt令牌被篡改，而正是因为jwt令牌最后一个部分数字签名的存在，所以整个jwt 令牌是非常安全可靠的。一旦jwt令牌当中任何一个部分、任何一个字符被篡改了，整个令牌在校验的时候都会失败，所以它是非常安全可靠的。

![image-20230106085442076](https://typora-picsbed-new.oss-cn-shanghai.aliyuncs.com/typora/image-20230106085442076.png?x-os)

> JWT是如何将原始的JSON格式数据，转变为字符串的呢？
>
> 其实在生成JWT令牌时，会对JSON格式的数据进行一次编码：进行base64编码
>
> Base64：是一种基于64个可打印的字符来表示二进制数据的编码方式。既然能编码，那也就意味着也能解码。所使用的64个字符分别是A到Z、a到z、 0- 9，一个加号，一个斜杠，加起来就是64个字符。任何数据经过base64编码之后，最终就会通过这64个字符来表示。当然还有一个符号，那就是等号。等号它是一个补位的符号
>
> 需要注意的是Base64是编码方式，而不是加密方式。



![image-20230112114319773](https://typora-picsbed-new.oss-cn-shanghai.aliyuncs.com/typora/image-20230112114319773.png?x-os) 

JWT令牌最典型的应用场景就是登录认证：

1. 在浏览器发起请求来执行登录操作，此时会访问登录的接口，如果登录成功之后，我们需要生成一个jwt令牌，将生成的 jwt令牌返回给前端。
2. 前端拿到jwt令牌之后，会将jwt令牌存储起来。在后续的每一次请求中都会将jwt令牌携带到服务端。
3. 服务端统一拦截请求之后，先来判断一下这次请求有没有把令牌带过来，如果没有带过来，直接拒绝访问，如果带过来了，还要校验一下令牌是否是有效。如果有效，就直接放行进行请求的处理。



在JWT登录认证的场景中我们发现，整个流程当中涉及到两步操作：

1. 在登录成功之后，要生成令牌。
2. 每一次请求当中，要接收令牌并对令牌进行校验。

稍后我们再来学习如何来生成jwt令牌，以及如何来校验jwt令牌。









### 2.3.2 生成和校验

简单介绍了JWT令牌以及JWT令牌的组成之后，接下来我们就来学习基于Java代码如何生成和校验JWT令牌。

首先我们先来实现JWT令牌的生成。要想使用JWT令牌，需要先引入JWT的依赖：

~~~xml
<!-- JWT依赖-->
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt</artifactId>
    <version>0.9.1</version>
</dependency>
~~~

> 在引入完JWT来赖后，就可以调用工具包中提供的API来完成JWT令牌的生成和校验
>
> 工具类：Jwts



生成JWT代码实现：

~~~java
@Test
public void genJwt(){
    Map<String,Object> claims = new HashMap<>();
    claims.put("id",1);
    claims.put("username","Tom");
    
    String jwt = Jwts.builder()
        .setClaims(claims) //自定义内容(载荷)          
        .signWith(SignatureAlgorithm.HS256, "itheima") //签名算法        
        .setExpiration(new Date(System.currentTimeMillis() + 24*3600*1000)) //有效期   
        .compact();
    
    System.out.println(jwt);
}
~~~

运行测试方法：

~~~
eyJhbGciOiJIUzI1NiJ9.eyJpZCI6MSwiZXhwIjoxNjcyNzI5NzMwfQ.fHi0Ub8npbyt71UqLXDdLyipptLgxBUg_mSuGJtXtBk
~~~

输出的结果就是生成的JWT令牌,，通过英文的点分割对三个部分进行分割，我们可以将生成的令牌复制一下，然后打开JWT的官网，将生成的令牌直接放在Encoded位置，此时就会自动的将令牌解析出来。

![image-20230106190950305](https://typora-picsbed-new.oss-cn-shanghai.aliyuncs.com/typora/image-20230106190950305.png?x-os)

> 第一部分解析出来，看到JSON格式的原始数据，所使用的签名算法为HS256。
>
> 第二个部分是我们自定义的数据，之前我们自定义的数据就是id，还有一个exp代表的是我们所设置的过期时间。
>
> 由于前两个部分是base64编码，所以是可以直接解码出来。但最后一个部分并不是base64编码，是经过签名算法计算出来的，所以最后一个部分是不会解析的。





实现了JWT令牌的生成，下面我们接着使用Java代码来校验JWT令牌(解析生成的令牌)：

~~~java
@Test
public void parseJwt(){
    Claims claims = Jwts.parser()
        .setSigningKey("itheima")//指定签名密钥（必须保证和生成令牌时使用相同的签名密钥）  
	    .parseClaimsJws("eyJhbGciOiJIUzI1NiJ9.eyJpZCI6MSwiZXhwIjoxNjcyNzI5NzMwfQ.fHi0Ub8npbyt71UqLXDdLyipptLgxBUg_mSuGJtXtBk")
        .getBody();

    System.out.println(claims);
}
~~~

运行测试方法：

~~~
{id=1, exp=1672729730}
~~~

> 令牌解析后，我们可以看到id和过期时间，如果在解析的过程当中没有报错，就说明解析成功了。



下面我们做一个测试：把令牌header中的数字9变为8，运行测试方法后发现报错：

> 原header： eyJhbGciOiJIUzI1NiJ9
>
> 修改为： eyJhbGciOiJIUzI1NiJ8

![image-20230106205045658](https://typora-picsbed-new.oss-cn-shanghai.aliyuncs.com/typora/image-20230106205045658.png?x-os)

结论：篡改令牌中的任何一个字符，在对令牌进行解析时都会报错，所以JWT令牌是非常安全可靠的。



我们继续测试：修改生成令牌的时指定的过期时间，修改为1分钟

~~~java
@Test
public void genJwt(){
    Map<String,Object> claims = new HashMap<>();
    claims.put(“id”,1);
    claims.put(“username”,“Tom”);
    String jwt = Jwts.builder()
        .setClaims(claims) //自定义内容(载荷)          
        .signWith(SignatureAlgorithm.HS256, “itheima”) //签名算法        
        .setExpiration(new Date(System.currentTimeMillis() + 60*1000)) //有效期60秒   
        .compact();
    
    System.out.println(jwt);
    //输出结果：eyJhbGciOiJIUzI1NiJ9.eyJpZCI6MSwiZXhwIjoxNjczMDA5NzU0fQ.RcVIR65AkGiax-ID6FjW60eLFH3tPTKdoK7UtE4A1ro
}

@Test
public void parseJwt(){
    Claims claims = Jwts.parser()
        .setSigningKey("itheima")//指定签名密钥
.parseClaimsJws("eyJhbGciOiJIUzI1NiJ9.eyJpZCI6MSwiZXhwIjoxNjczMDA5NzU0fQ.RcVIR65AkGiax-ID6FjW60eLFH3tPTKdoK7UtE4A1ro")
        .getBody();

    System.out.println(claims);
}
~~~

等待1分钟之后运行测试方法发现也报错了，说明：JWT令牌过期后，令牌就失效了，解析的为非法令牌。

通过以上测试，我们在使用JWT令牌时需要注意：

- JWT校验时使用的签名秘钥，必须和生成JWT令牌时使用的秘钥是配套的。

- 如果JWT令牌解析校验时报错，则说明 JWT令牌被篡改 或 失效了，令牌非法。 





### 2.3.3 登录下发令牌

JWT令牌的生成和校验的基本操作我们已经学习完了，接下来我们就需要在案例当中通过JWT令牌技术来跟踪会话。具体的思路我们前面已经分析过了，主要就是两步操作：

1. 生成令牌
   - 在登录成功之后来生成一个JWT令牌，并且把这个令牌直接返回给前端
2. 校验令牌
   - 拦截前端请求，从请求中获取到令牌，对令牌进行解析校验



那我们首先来完成：登录成功之后生成JWT令牌，并且把令牌返回给前端。



JWT令牌怎么返回给前端呢？此时我们就需要再来看一下接口文档当中关于登录接口的描述（主要看响应数据）：

- 响应数据

  参数格式：application/json

  参数说明：

  | 名称 | 类型   | 是否必须 | 默认值 | 备注                     | 其他信息 |
  | ---- | ------ | -------- | ------ | ------------------------ | -------- |
  | code | number | 必须     |        | 响应码, 1 成功 ; 0  失败 |          |
  | msg  | string | 非必须   |        | 提示信息                 |          |
  | data | string | 必须     |        | 返回的数据 , jwt令牌     |          |

  响应数据样例：

  ~~~json
  {
    "code": 1,
    "msg": "success",
    "data": "eyJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoi6YeR5bq4IiwiaWQiOjEsInVzZXJuYW1lIjoiamlueW9uZyIsImV4cCI6MTY2MjIwNzA0OH0.KkUc_CXJZJ8Dd063eImx4H9Ojfrr6XMJ-yVzaWCVZCo"
  }
  ~~~

- 备注说明

  用户登录成功后，系统会自动下发JWT令牌，然后在后续的每次请求中，都需要在请求头header中携带到服务端，请求头的名称为 token ，值为 登录时下发的JWT令牌。

  如果检测到用户未登录，则会返回如下固定错误信息：

  ~~~json
  {
  	"code": 0,
  	"msg": "NOT_LOGIN",
  	"data": null
  }
  ~~~

解读完接口文档中的描述了，目前我们先来完成令牌的生成和令牌的下发，我们只需要生成一个令牌返回给前端就可以了。





**实现步骤：**

1. 引入JWT工具类
   - 在项目工程下创建com.itheima.utils包，并把提供JWT工具类复制到该包下
2. 登录完成后，调用工具类生成JWT令牌并返回



**JWT工具类**

~~~java
public class JwtUtils {

    private static String signKey = "itheima";//签名密钥
    private static Long expire = 43200000L; //有效时间

    /**
     * 生成JWT令牌
     * @param claims JWT第二部分负载 payload 中存储的内容
     * @return
     */
    public static String generateJwt(Map<String, Object> claims){
        String jwt = Jwts.builder()
                .addClaims(claims)//自定义信息（有效载荷）
                .signWith(SignatureAlgorithm.HS256, signKey)//签名算法（头部）
                .setExpiration(new Date(System.currentTimeMillis() + expire))//过期时间
                .compact();
        return jwt;
    }

    /**
     * 解析JWT令牌
     * @param jwt JWT令牌
     * @return JWT第二部分负载 payload 中存储的内容
     */
    public static Claims parseJWT(String jwt){
        Claims claims = Jwts.parser()
                .setSigningKey(signKey)//指定签名密钥
                .parseClaimsJws(jwt)//指定令牌Token
                .getBody();
        return claims;
    }
}

~~~



**登录成功，生成JWT令牌并返回**

~~~java
@RestController
@Slf4j
public class LoginController {
    //依赖业务层对象
    @Autowired
    private EmpService empService;

    @PostMapping("/login")
    public Result login(@RequestBody Emp emp) {
        //调用业务层：登录功能
        Emp loginEmp = empService.login(emp);

        //判断：登录用户是否存在
        if(loginEmp !=null ){
            //自定义信息
            Map<String , Object> claims = new HashMap<>();
            claims.put("id", loginEmp.getId());
            claims.put("username",loginEmp.getUsername());
            claims.put("name",loginEmp.getName());

            //使用JWT工具类，生成身份令牌
            String token = JwtUtils.generateJwt(claims);
            return Result.success(token);
        }
        return Result.error("用户名或密码错误");
    }
}
~~~



重启服务，打开postman测试登录接口：

