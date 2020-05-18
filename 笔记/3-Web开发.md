# 3. Web
## 3.1. servlet
### 3.1.1. Http协议
请求：客户端发送数据给服务器</br>
响应：服务器把处理结果给客户端</br>
url包含了服务器地址和请求让服务器干什么</br>

服务器要处理请求，不同人发过来的格式不同，因此需要遵循一个协议。要规范浏览器和服务器的数据交互格式。

#### 3.1.1.1. HTTP的概念和介绍
全名：超文本传输协议，规范了交互格式。</br>
通过键值对可以精准的描述。</br>
特点：

- 简单快速：客户想服务器请求服务时，只需传送请求方法和路径。通过键值对发送数据。常用的有GET/HEAD/POST
- 灵活：允许传输任意类型的数据对象。正在传输的类型由Content-Type加以标记
- 无连接：无连接的含义是限制每次连接只处理一个请求。处理完客户的请求，并完成应答后，就会断开连接。可以节省传输时间。**HTTP1.1版本后支持可持续连接**，一定时间无反应后，才会断开
- 无状态：协议对于事务处理没有记忆能力。 只规定格式，跟具体是什么数据没关系。

#### 3.1.1.2. HTTP的交互流程
一个HTTP消息包括四个步骤：

1. 客户端和服务器建立连接
2. 客户端发送请求数据到服务端（HTTP）
3. 服务器端接收到请求后，进行处理，然后将处理结果相应客户端（HTTP）
4. 关闭客户端和服务器端的链接（HTTP1.1后不会立即关闭）

#### 3.1.1.3. HTTP请求格式
请求格式的结构：

- 请求头：请求方式(GET/POST)、请求的地址和HTTP协议版本
- 请求行：消息报头，一般用来说明客户端要试用的一些附加信息（没有用户个人信息，是附带信息，告诉服务器应该怎么响应。）
- 空行：位于请求行和请求数据之间，空行是必须的。
- 请求数据：非必须。

全部都是键值对

GET请求方式没有请求主体。POST请求方式有请求主体。

### 3.1.2. HTTP请求方式
HTTP1.0定义了三种请求方式：GET/POST/HEAD</br>
HTTP1.1定义了五中请求方式：OPTIONS,PUT,DELETE,TRACE和CONNECT

- GET 请求指定的页面信息，并返回实体主体
- HEAD 类似于GET请求，只不过返回的响应中没有具体的内容，用于获取报头
- POST 向指定资源提交数据进行处理请求。

请求方式是网页声明的。

GET的请求方式请求数据被拼接在了请求头中的地址里。所以请求数据是空的。不安全，只能传少量数据，因为地址栏是有限的。</br>
POST的请求数据是单独存放的，不限定上传数据量，安全的，适合数量达的数据发送。

#### 3.1.2.1. HTTP协议的响应
响应格式的结构：

- 相应行（状态行）：HTTP版本、状态码、状态消息
- 相应头：消息报头，客户端使用的附加信息（用什么格式解析服务器给的数据，大小，类型）
- 空行：必须的
- 响应实体：正文，服务器返回给浏览器的信息

> HTTP常见响应状态码含义：
> HTTP状态码由三个十进制数字组成，第一个十进制数字定义了状态码的类型，后两个数字没有分类的作用。状态码共分为5类

常见的状态码：

- 200 OK //客户端请求成功
- 400 Bad Request // 客户端请求有语法错误，不能被服务器所理解
- 401 Unauthorized // 请求未经授权，这个状态代码必须和WWW-Authenticate报头域一起使用
- 403 Forbidden // 服务器收到请求，但是拒绝提供服务
- 404 notFound // 请求资源不存在，比如输入了错误的URL
- 500 Internal Server Error // 服务器发生不可预期的错误
- 503 Server Unavailable // 服务器当前不能处理客户端的请求，一段时间后恢复正常

### 3.1.3. Tomcat服务器介绍和使用

目前学的java代码只有一次性，也就是运行完毕后，如果需要再次运行则需要再次动手启动代码的执行。因此，我们不知道用户何时会启动，我们的代码何时运行。

我们需要写一个容器，该容器可以根据用户的请求来启动并运行我们编写的数据逻辑代码。服务器就是这个容器。

所谓服务器就是代码编写的一个可以根据用户请求实时的调用执行对应的逻辑代码的一个容器。将实现写好的代码放到服务器的指定位置，启动服务器，那么服务器就自动的会根据接收到请求调用并执行对象的逻辑代码进行处理。

> 我的tomcat在/Library/Tomcat/bin
> 使用 sudo sh ./startup.sh启动
> 使用 sudo ./shutdown.sh

写好的代码放在./webapps/中
./conf/server.xml 中可以修改端口号

#### 3.1.3.1. 第一个web程序
[idea配置tomcat和servlet](https://www.cnblogs.com/m-yb/p/10925688.html)
#### 3.1.3.2. servlet的介绍
服务器根据请求，并调用哪个类和哪个方法来进行处理。
在编写代码的时候就要按照服务器能够识别的规则编写，浏览器按照指定的规则进行发送请求，服务器就可以调用并执行响应的逻辑代码进行请求处理。

服务器收到一个请求，不管请求是什么，去java类中找一个service方法，找到了就调用它。自己写的类要继承HttpServlet，然后重写service函数即可。

用servlet写的代码能让Tomcat服务器认识。狭义的servlet是指java语言实现的一个接口。广义的servlet是指任何实现了这个servlet接口的类。

特点：运行在支持java的应用服务器上。servlet的实现遵循了服务器能够识别的规则。

servlet使用：

- 创建普通的java类并继承HttpServlet
- 复写service方法
- 在service方法中书写逻辑代码即可
- 在webRoot下的WEB-INF文件夹下的web.xml中配置servlet

URL的组成： 服务器地址:端口号/虚拟项目名/servlet的别名（url-pattern）

运行流程：浏览器发送请求到服务器，服务器根据请求URL地址中的URI信息在webapps目录下找到对应的项目文件夹，然后再web.xml中检索对应的servlet，找到后调用并执行Servlet。

#### 3.1.3.3. servlet生命周期
自动配置servlet，在package地方创建servlet。
运行的是内存里的东西。运行之后，把文件删除还是不会报错。直到关闭服务器，重启才会报错。 

第一次对servlet调用时，加载进内存。init()方法只会在第一次servlet加载内存的时候执行一次。

servlet只要加载进了tomcat内存，必须一直在，除非关闭了服务器。

服务器关闭时，自动调用destroy方法。在servlet被销毁时执行。

**如果在web.xml中配置了load-on-startup。那么init会在服务器启动的时候就init()。否则是在第一次访问时init()。**
```
package com.wyj.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class MyServlet extends HttpServlet {
    // 初始化方法，在servlet第一次加载内容的时候会被调用
    @Override
    public void init() throws ServletException{
        System.out.println("servlet初始化完成");
    }

    // service 方法整整处理请求的方法
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.getWriter().write("this is my first servlet");
        System.out.println("this is my first servlet");
    }

    @Override
    public void destroy(){
        System.out.println("我被销毁了");
    }
}

////////////////////////web.xml
<servlet>
            <servlet-name>my</servlet-name>
            <servlet-class>com.wyj.servlet.MyServlet</servlet-class>
            <load-on-startup>1</load-on-startup>
        </servlet>
        <!-- 配置访问方式 这里servlet-name与上面要一致，随意取-->
        <servlet-mapping>
            <servlet-name>my</servlet-name>
            <url-pattern>/my</url-pattern>
        </servlet-mapping>
```

#### 3.1.3.4. doGet方法和doPost方法
**jsp修改不需要重启服务器，但是修改servlet需要重启**
不管是post还是get方法，service都能处理。

post要和post对应，get要和get对应，否则405。

如果servlet中有service，有先调用service，不管是post还是get方法。如果service方法中有`super.service(req, resp);`这句话，会同时调用service（先）与doPost或doGet（后）。可以在service中判断get和post分开始处理，调用相应的doGet或doPost。
```
package com.wyj.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/**
 *
 * Service方法和doGet方法和doPost方法的区别
 * service：
 *      get和post都可以
 *      如果servlet中同时有service和doGet或doPost。不管是
 *      post还是get有先用service
 *
 *      如果有这句话super.service(req, resp);会同时执行
 *      service（先用）和doPost或doGet（后用）。
 *      可以在service中判断get和post分开始处理
 * doGet:
 *      处理get请求
 * doPost:
 *      处理post请求
 */

@WebServlet(name = "ServletMethod")
public class ServletMethod extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("我是service");
        super.service(req, resp);
    }

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.print("我是get");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("我是post");
    }
}
```

#### 3.1.3.5. servlet的常见错误：
- 404错误：资源未找到
    - 在请求地址中的servlet的别名写错了
    - 虚拟项目名称拼写错误
- 500错误：内部服务器错误
    - web.xml中校验类的全限定路径是否拼写错误
    - service方法有逻辑错误。
- 405错误：请求方式不支持
    - 请求方式和servlet中的方法不匹配所造成的。尽量用service方法进行处理，不要在service中调用super

#### 3.1.3.6. request对象的介绍和获取的请求头
来一次请求，创建一个对象，储存请求数据。服务器在调用servlet时会将创建的Request对象作为实参传递给servlet的方法。 
```
package com.wyj.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/**
 *
 * Service方法和doGet方法和doPost方法的区别
 * service：
 *      get和post都可以
 *      如果servlet中同时有service和doGet或doPost。不管是
 *      post还是get有先用service
 *
 *      如果有这句话super.service(req, resp);会同时执行
 *      service（先用）和doPost或doGet（后用）。
 *      可以在service中判断get和post分开始处理
 * doGet:
 *      处理get请求
 * doPost:
 *      处理post请求
 */

@WebServlet(name = "ServletMethod")
public class ServletMethod extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("我是service");
        super.service(req, resp);
    }

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.print("我是get");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("我是post");
    }
}
```

```
package com.wyj.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.Enumeration;

/**
 * request 对象中封存了当前请求的所有请求信息
 * 由Tomcat服务器创建，并作为实参传递给servlet的service方法
 *
 * 获取请求头的方法：
 *      req.getMethod(); 获取请求方式
 *      req.getRequestURL(); 获取url
 *      req.getRequestURI(); 获取uri
 *      req.getScheme(); 获取协议
 * 获取请求行数据:
 *      req.getHeader("User-Agent"); 返回指定请求头信息
 *      req.getHeaderNames(); 返回请求头的键名的枚举
 * 获取用户数据：
 *      req.getParameter("uname"); //不能获取同键不同值的多选
 *      req.getParameterValues("fav"); //同键多值，返回一个String数组
 *      req.getParameterNames(); // 返回所有用户请求数据的枚举集合
 *      不存在不会报错，返回null，要做好判断
 */

@WebServlet(name = "RequestServlet")
public class RequestServlet extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 获取请求头
        // 获取请求方式
        String method = req.getMethod();
        // 获取URL
        StringBuffer url = req.getRequestURL();
        // 获取协议
        String scheme = req.getScheme();

        // 获取请求行 通过键值对的方式获取
        // 如果没有这个键 返回null
        String value = req.getHeader("User-Agent");
        // 获取所有的请求行键的枚举
        Enumeration e = req.getHeaderNames();
        while(e.hasMoreElements()){
            String name = (String) e.nextElement();
            String value2 = req.getHeader(name);
            System.out.println(name+":"+value2);
        }

        // 获取用户数据 不区分是get还是post
        String name = req.getParameter("uname");
        String pwd = req.getParameter("pwd");
        String[] favs = req.getParameterValues("fav");
        if(favs!=null){
            for(String fav:favs)
                System.out.println(fav);
        }


    }
}

```

#### 3.1.3.7. Response对象
如何显示到浏览器。需要使用Response对象。使用对象+I/O流来写。服务器在调用指定的Response时，
```
package com.wyj.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/**
 *
 * Response 响应数据到浏览器的对象
 * 设置响应头
 *      resp.setHeader("mouse", "shuangfeiyan");
 *      resp.addHeader("key", "thinkpad");
 * 设置响应编码
 *      resp.setContentType("text/html;charset=utf-8");
 * 设置响应状态
 *      send.Error(int num, String msg);
 * 设置响应实体
 *      resp.getWriter().write("<b>我爱学习</b>");
 *
 * 总结：
 *      1.获取请求
 *      2.处理请求数据
 *          数据库操作MVC思想
 *      3.设置响应编码格式
 *      4.响应处理结果
 */

@WebServlet(name = "ResponseServlet")
public class ResponseServlet extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 获取请求信息
        // 获取请求头
        // 获取请求行
        // 获取用户数据


        // 处理请求


        //响应处理结果
        // 设置响应头
        resp.setHeader("mouse", "shuangfeiyan"); // 覆盖，只能设置一个值
        resp.addHeader("key", "thinkpad"); // 新加

        // 设置响应编码格式 允许使用中文 一下两句效果一样
//        resp.setHeader("content-type", "text/html;charset=utf-8");
        resp.setContentType("text/html;charset=utf-8");
        // 设置响应状态码
//        resp.sendError(404, "Sorry"); // 报错404

        // 设置响应体
        resp.getWriter().write("<b>我爱学习</b>");
    }
}
```

### 3.1.4. 登录练习
【Learning项目】
需要的知识：MVC设计模式，数据库知识JDBC

1. 创建登录页面
    1. 创建servlet进行登录页面请求处理
2. 点击完成登录操作
    1. 浏览器发送请求到服务器（用户信息+其他数据）
    2. 服务器调用对应的servlet进行处理
        1. 设置响应编码格式
        2. 获取请求信息
        3. 处理请求信息
        4. 响应处理结果
3. 在servlet中完成用户登录校验
    1. 需要连接数据库（在mysql中创建用户表）
4. 使用MVC思想完成

servlet拿到用户名密码，传给service，service传给DAO,用户名密码给DAO层，去数据查是否有这个人，把查到的User封装成对象，返回给业务层service层，service层返回给servlet层。判断是否有这个人。

#### 3.1.4.1. 请求乱码的解决
响应网页中的乱码可以通过`resp.setContentType("text/html;charset=utf-8");`解决。
但是请求数据会出现乱码。需要修改编码。
- 方法1
    - `uname = new String(uname.getBytes("iso8859-1"),"UTF-8");`
- 方法2 使用公共配置 与请求方式有关
    - post方式 `req.setCharacterEncoding("UTF-8");` 
    - get方式 除了加上post那句话 还要在tomcat服务器目录下conf文件中找到server.xml 中 `<Connector port="8080" protocol="HTTP/1.1"...`最后加上`useBodyEncodingForUri="true"`

**注意：tomcat9以后默认为utf-8**不再需要修改

#### 3.1.4.2. Servlet流程总结
- 浏览器发起请求到服务器（请求）
- 服务器接收浏览器的请求，进行解析，创建request对象存储请求数据。
- 服务器调用对应的Servlet进行请求处理，并用request对象作为实参传递给servlet的方法
- servlet的方法执行请求处理
    - 设置请求编码格式
    - 设置响应编码格式
    - 获取请求信息
    - 处理请求信息
        - 创建业务层对象 Service
        - 调用业务层对象的方法 Service
    - 响应处理结果

#### 3.1.4.3. 数据流转流程
浏览器->服务器->数据库->服务器->浏览器    

#### 3.1.4.4. 请求转发
第一个参数为要转发的地址，url-pattern
`req.getRequestDispatcher("page").forward(req,resp);`

作用：实现多个servlet联动操作处理请求，避免代码冗余，让servlet的职责更加明确。

特点：一次请求，浏览器地址栏信息不改变。

注意：请求转发后直接return即可。

#### 3.1.4.5. request对象的作用域
一次请求内的sevlet共享request作用域。不请求转发的话只有一个servlet，如果请求转发则2个以上。

可能第二个sevlet需要第一个sevlet的处理结果，不同sevlet之间的数据流转。第一个sevlet处理完，可以存到req中，传到第二个去。

解决了一次请求内不同对象的数据共享问题

[关于req.getAttribute和req.getParameter]
- 如果是两个Web页面间为链接(重定向)关系时，如用method=get  / method=post 表单提交请求，传递请求参数。就是说要从    1.jsp链接到2.jsp时，被链接的是2.jsp可以通过getParameter()方法来获得请求参数。此种方法是从web客户端向web服务端传递数据，代表HTTP请求数据
- 如果两个Web间为转发关系时，转发目的地web可以用getAttribute()方法来和转发源Web共享request域内的数据。此种方法只存在于web容器内部


```
///////////////////LoginServlet.java
package com.wyj.Servlet;

import com.wyj.pojo.User;
import com.wyj.service.LoginService;
import com.wyj.service.impl.LoginServiceImpl;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.Enumeration;

@WebServlet(name = "LoginServlet")
public class LoginServlet extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 设置响应编码格式
        resp.setContentType("text/html;charset=utf-8");
        // 获取请求信息
        String uname = req.getParameter("uname");
        // 为了支持中文，防止出现乱码 但是tomcat9以后默认为utf-8
//        uname = new String(uname.getBytes("iso8859-1"),"UTF-8");
        String pwd = req.getParameter("pwd");
        System.out.println(uname + ":" + pwd);

        // 处理请求信息
        // 获取业务层对象
        LoginService ls = new LoginServiceImpl();
        User u = ls.checkLoginService(uname, pwd);
        System.out.println(u);
        // 响应处理结果
        if(u!=null){
            resp.getWriter().write("登录成功");
        }
        else{
            // 使用request对象实现不同servlet的数据流转
            // 只有从login流转到page的才会有这句话。直接访问page的没有这句话
            req.setAttribute("str", "用户名或密码错误");
            // 使用请求转发回登录页面 要把req和resp转发下去，下一个servlet才能用
            req.getRequestDispatcher("page").forward(req,resp);
            return;
        }
    }
}

////////////////////PageServlet.java
package com.wyj.Servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet(name = "PageServlet")
public class PageServlet extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 设置响应编码格式
        resp.setContentType("text/html;charset=utf-8");
        // 获取请求信息
        // 处理请求
        // 响应处理结果

        // 获取做request作用域数据 下面这句话返回的是Object 要强转
        // 但是一开始是直接访问page，没有这个东西，会显示null，只有内部请求转发过来的才能显示
        String str = (String) req.getAttribute("str");

        resp.getWriter().write("<html>");
        resp.getWriter().write("<head>");
        resp.getWriter().write("</head>");
        resp.getWriter().write("<body>");

        // 判断，只有非null是请求转发过来的，才能显示
        if(str != null)
            resp.getWriter().write("<font color='red' size='20px'>" + str + "</font>");

        // action 就是xml里的url—pattern 这里意味着跳转的url-pattern
        // 比如这里就是按了登录之后，跳转的页面，先从page输入信息，点击登录跳转到login
        resp.getWriter().write("<form action='login' method='get'><br/>");
        resp.getWriter().write("用户名：<input type='text' name='uname' value=''/><br/>");
        resp.getWriter().write("密码：<input type='password' name='pwd' value=''/><br/>");
        resp.getWriter().write("<input type='submit' value='登录'/><br/>");
        resp.getWriter().write("</form>");
        resp.getWriter().write("</body>");
        resp.getWriter().write("</html>");
    }
}

```

#### 3.1.4.6. 重定向
如果用请求转发，地址栏信息不改，还是登陆请求的url，如果用get，url中有账号密码，刷新mainServlet页面时，登陆信息会重复提交。
`http://localhost:8080/Learning_war_exploded/login?uname=zhangsan&pwd=123`

如果请求中的表单数据可以重复使用，可以用请求转发，但是如果数据不能重复提交，需要用重定向。

重定向会有多个请求，先处理login请求，然后让sevlet发一个新的请求，去main。将丢失信息，无法再访问uname。将显示为null。

- 解决了表单重复提交的问题，以及当前sevlet无法处理的请求问题
- `resp.sendRedirect("/Learning_war_exploded/main");` 里面填的是URI地址 虚拟项目名+servlet
- 两次请求，地址栏信息改变，两个req对象
- 如果请求中有表单数据，而数据比较重要，不能重复提交建议用重定向。
- 如果请求被servlet接收后，无法进行处理，建议使用重定向到可以处理的资源。

**如何让不同请求拿到同一个对象。session可以解决，session依赖于cookie。**

**路径重点**
重定向路径总结：
相对路径：从当前请求的路径查找资源的路径
    相对路径如果servlet的别名中包含目录，会造成重定向资源查找失败。
绝对路径：第一个/表示服务器根目录。就是/表示localhost:8080/
    /虚拟项目名/资源路径    
    
请求转发路径总结：
/表示项目根目录。就是/表示localhost:8080/ManagerSystem/
推荐用法：/资源路径    

### 3.1.5. cookie介绍和使用
#### 3.1.5.1. cookie存储
【Cookie项目】
cookie解决发送的不同请求的数据共享问题。第一次发的数据，第二次可能会用到。
```
    // 使用cookie进行浏览器端的数据存储
        Cookie c = new Cookie("mouse", "thinkpad");
            // 添加cookie信息
        resp.addCookie(c);
```
之后请求该项目就会带上cookie
注意：一个cookie对象存储一条数据，多条数据可以多创建几个cookie对象，进行存储。

特点：
- 特点：cookie技术是浏览器端的技术，声明在服务器。
- 临时存储：cookie数据存在浏览器运行内存中。浏览器关闭则失效
- 定时存储：设置cookie有效期`c2.setMaxAge(3*24*60*60);`cookie存到了客户端的硬盘中，在有效期内符合路径要求的请求都会附带该信息。
- 默认：cookie信息存储好之后，项目每次请求都会附带，除非设置有效路径

```
package com.wyj.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet(name = "CookieServlet")
public class CookieServlet extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 设置请求编码格式
        // 设置响应编码格式
        resp.setContentType("text/html;charset=utf-8");
        // 获取请求信息
        // 处理请求信息
        // 响应处理结果
            // 使用cookie进行浏览器端的数据存储
        Cookie c = new Cookie("mouse", "thinkpad");
        Cookie c2 = new Cookie("key", "keyboard");
        //设置cookie 可选
            // 设置有效期 参数以秒为单位
        c2.setMaxAge(3*24*60*60);
            // 设置有效路径 只有在这个有效路径下，才会附带cookie信息
        c2.setPath("/Cookie_war_exploded/gc");
            // 添加cookie信息
        resp.addCookie(c);
        resp.addCookie(c2);
            // 直接响应
        resp.getWriter().write("Cookie学习");
            // 请求转发
            // 重定向
    }

}
```

#### 3.1.5.2. cookie获取
【Cookie项目】
```
package com.wyj.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet(name = "GetCookieServlet")
public class GetCookieServlet extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 设置请求编码
        // 设置响应编码格式
        resp.setContentType("text/html;charset=utf-8");
        // 获取请求信息
            // 获取cookie信息 小心无cookie情况下的访问 要小心null
            // 获取cookie数组
        Cookie[] cks = req.getCookies();
        if(cks!=null){
            // 遍历cookie数组
            for(Cookie c : cks){
                String name = c.getName();
                String value = c.getValue();
                System.out.println(name+":"+value);
            }
        }
            // 获取用户数据
        // 处理请求信息
        // 响应处理结果
            // 直接响应
            // 请求转发
            // 重定向
    }
}

```

### 3.1.6. cookie使用之三天免登陆
[Learning项目]
第一次登陆login时，不仅重定向到main，还把个人信息保存到本地cookie中。

可以把cookie校验的servlet作为网页的入口。如果有cookie且正确，直接进入main，否则去page登陆

```
//////////////////CookieServlet.java
package com.wyj.Servlet;

import com.wyj.pojo.User;
import com.wyj.service.LoginService;
import com.wyj.service.impl.LoginServiceImpl;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/**
 * 作用：判断是否携带正确的cookie信息
 * 如果有则校验cookie是否正确
 *      如果正确，则直接响应主页面
 *      不正确，则响应登陆页面
 *   如果没有，则请求转发给登陆页面
 */

@WebServlet(name = "CookieServlet")
public class CookieServlet extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 设置响应编码
        resp.setContentType("text/html;charset:utf-8");
        // 获取请求信息
        Cookie[] cks = req.getCookies();
        if(cks!=null){
            // 遍历cookie信息
            String uid = "";
            for(Cookie c:cks){
                if("uid".equals(c.getName())){
                    uid = c.getValue();
                }
            }
            // 校验uid是否存在
            if("".equals(uid)){
                req.getRequestDispatcher("page").forward(req,resp);
                return;
            }
            // 校验uid信息
            else{
                // 获取业务层对象
                LoginService ls = new LoginServiceImpl();
                User u = ls.checkLoginService(uid);
                if(u!=null){
                    // 数据库中有 则直接进入主页面
                    resp.sendRedirect("/Learning_war_exploded/main");
                }
                else{
                    // 请求转发给page
                    req.getRequestDispatcher("page").forward(req,resp);
                }
            }
        }else{
            // 请求转发给page
            req.getRequestDispatcher("page").forward(req,resp);
        }
        // 处理请求信息
        // 响应处理结果

    }
}
```

### 3.1.7. Session学习
req只能是一次请求的数据共享。
session可以让一个用户发起的不同请求拿到同一个对象。

[原理]
用户第一次访问服务器，服务器会创建一个session对象给此用户，并将session对象的KSESSIONID使用cookie技术存储到浏览器中，保证用户的其他请求能够获取到同一个session对象，保证不同请求能够获取到共享的数据。

[特点]
存储在服务器端，由服务器进行创建，依赖Cookie技术。有效期是一次会话（即浏览器的关闭）。默认存储时间是30分钟。

[使用]
创建/获取session对象
`HttpSession hs = req.getSession();`
如果有了则返回对应的session。
如果没有就创建，其JSESSIONID数据作为从cookie存入数据。
如果session失效了，也会重新创建一个session。

设置session存储时间
`hs.setMaxInactiveInterval(5);`
注意：五秒内session对象没有被使用则销毁，如果使用了则重新计时。
也可以在tomcat/conf/web.xml中改，<session-config>中改。

设置session强制失效
`hs.invalidate();`

[注意]
JSESSIONID存储在了cookie临时存储空间中，浏览器关闭即失效。

 ```
 package com.wyj.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;

public class SessionServlet extends javax.servlet.http.HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 设置响应编码格式
        resp.setContentType("text/html;charset=utf-8");
        // 接收请求信息
        String name = "张三";
        // 创建session，如果有session就获取，如果没有session就创建
        HttpSession hs = req.getSession();
        // 设置session的存储时间 参数为秒
        hs.setMaxInactiveInterval(5);
        System.out.println(hs.getId());
        // 强制session失效，比如点击退出的时候
        hs.invalidate();
        // 处理请求信息
        // 响应处理结果
            // 直接响应
        resp.getWriter().write("session链接");
            // 请求转发
            // 重定向
    }
}
 ```

#### 3.1.7.1. session的数据流转和总结
```
HttpSession hs = req.getSession();
System.out.println(hs.getAttribute("name"));
```

存储：`hs.setAttribute(String name, Object value);`
获取：`hs.getAttribute(String name);` 返回的是object对象

存储要先与获取，可以发生在不同的请求中。

[使用时机]
一般用户在登录web项目时，会将用户的登录信息存储到session中，供该用户的其他请求使用。

[失效情况]
1. 用户关闭了浏览器。这个不需要管，因为他还要重新登录
2. 用户没关浏览器，session失效了。**需要将用户请求中的JSESSIONID和后台的JSESSIONID进行对比，如果一样则继续，不一样则需要重定向到登录页面，重新登录。**

[总结]
session解决了一个用户的不同请求的数据共享问题，只要在JSESSIONID不失效和session对象不失效的情况下，用户的任意请求在处理时都能获取到同一个session对象。

### 3.1.8. ServletContext
request解决了一次请求内的数据共享，session解决了用户不同请求的数据共享，ServletContext对象可以解决不同用户的数据共享问题。

[原理]
ServletCoontext对象由服务器进行创建，一个项目只有一个。

[生命周期]
服务器启动到服务器关闭

[作用域]
项目内

[使用]
获取ServletContext对象
```
// 获取ServletContext对象的三种方法 效果一样
        // 第一种方式
        ServletContext sc = this.getServletContext();
        // 第二种方式
        ServletContext sc2 = this.getServletConfig().getServletContext();
        // 第三种方式
        ServletContext sc3 = req.getSession().getServletContext();
```
使用ServletContext完成数据共享
`sc.setAttribute("str", "ServletContext数据共享");`
`sc.getAttribute("str"); 返回Obj`

获取项目中web.xml文件中的全局配置数据
`String str = sc.getInitParameter("name");` 如果数据不存在返回null
`sc.getInitParameterNames();`返回键名的枚举

web.xml配置方式
配置全局静态的数据
作用：将静态数据和代码进行解耦
```
<context-param>
        <param-name>name</param-name>
        <param-value>zhangsan</param-value>
    </context-param>
```

获取项目webroot下的资源绝对路径
`String path = sc.getRealPath(String path);

获取项目根目录下资源的流对象
`InputStream is = sc.getResourceAsStream("/web/....");`

[注意]
不同的用户可以给ServletContext对象进行数据的存取。获取数据不存在返回null

一旦服务器关闭，ServletContext东西存不下来，可以在根目录下创建一个文件，来记录这个ServletContext数据，启动的时候在读进去

#### 3.1.8.1. ServletConfig对象
ServletConfig对象是Servlet的专属配置对象，每个Servlet都单独拥有一个ServletConfig对象，用来获取Web.xml中给每个servlet单独的配置信息。

[使用]
```
<servlet>
        <servlet-name>scg</servlet-name>
        <servlet-class>com.wyj.servlet.ServletConfigServlet</servlet-class>
        <!-- 配置局部的数据 放在servlet标签内-->
        <init-param>
            <param-name>config</param-name>
            <param-value>utf-8</param-value>
        </init-param>
    </servlet>
    <servlet-mapping>
        <servlet-name>scg</servlet-name>
        <url-pattern>/scg</url-pattern>
    </servlet-mapping>
```

```
ServletConfig sc = this.getServletConfig();
        // 获取web.xml中的数据
        sc.getInitParameter("config");
```

#### 3.1.8.2. web.xml和server.xml
##### 3.1.8.2.1. web.xml
[作用]
存储项目相关的配置信息，保护servlet，解耦一些数据对程序的依赖

[使用位置]
每个web项目中，tomcat服务器中（服务器目录下conf目录中）

[区别]
- web项目下的web.xml文件为局部配置，针对本项目
- tomcat下的全局配置
- 如果都配了，优先web项目下的

[内容（核心组件）]
- 全局上下文配置（全局配置参数）
- servlet配置
- 过滤器配置
- 监听器配置

[加载顺序]
ServletContext -> context-param -> listener -> filter -> servlet

项目内的web.xml加载时机是服务器启动的时候。

#### 3.1.8.3. server.xml
- Server
    - Service
        - Connector
            - 8080 http协议
            - 8009 AJP协议 服务器集群的时候用
        - Engine
            - Host 放项目的位置 与localhost名字
                - 可以做热部署<Context path="/Pet" reloadable="true" docBase="地址" /> 每次修改代码不重启服务器叫做热部署，项目绝对路径是docBase，path是项目的虚拟项目名称。如果把项目删了，一启动服务器就会报错。

### 3.1.9. jsp
Servlet进行页面的展现，代码写起来很麻烦。可以根据逻辑动态显示信息，比html好，html都是写死的。

使用jsp（Java Server Pages）可以保留sevlet的动态优点的同时，可以像html一样写起来方便。

[特点]
- 本质上是servlet
- 跨平台，一次编写处处运行
- 组件跨平台
- 健壮性和安全性

jsp配置在tomcat的xml中，*.jsp

#### 3.1.9.1. 访问原理
**把jsp其实就是servlet，jsp -> jspServlet -> servlet**

浏览器发起请求，请求jsp，请求被tomcat服务器接收，执行JspServlet，将请求的jsp文件转义成为对应的java（Servlet），然后执行转义好的java文件

[注意]
**tomcat并不认识.jsp文件！是根据上面的流程走的，tomcat只认识servlet**

#### 3.1.9.2. 语法和指令
```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" pageEncoding="utf-8"%>
<%--
这个是jsp的注释。
    Jsp的三种注释：
        前段语言注释<!-- -->
            会被转义，也会被发送，但是不会被浏览器执行
        Java语言注释
            会被转义，但是不会被servlet执行
        Jsp注释
            不会被转义
    Jsp的page指令学习：
        <%@page 属性名="属性值 "属性名="属性值" ... %>
        language: 声明jsp要被转义的语言
        import: 声明转义的java文件要导入的包，不同的包使用逗号隔开。
        pageEncoding: 当前jsp文件保存的编码格式，如果文件里有中文要设置为"utf-8"，同时也会设置浏览器解析格式
        contentType: 设置浏览器解析格式，高版本的jsp不需要写这个了，在pageEncoding里完成了
        session: 设置转义的servlet中是否开启session支持，默认为true开启。
        errorPage: 当前jsp有编译能过，但是出行报错时要跳转的页面
        extends: 设置jsp转义的java文件要继承的父类（包名+类名）
        作用：
            配置jsp文件的转义相关的参数
    Jsp的局部代码块：
        特点：
            局部代码块中声明的java代码会被原样转移到jsp对应的servlet文件的_JspService方法中
            代码块中声明的变量都是局部变量。
        使用：
            <% java代码 %>
        缺点：
            使用局部代码块在jsp中进行逻辑判断，书写麻烦，阅读困难。
        开发：
            使用servlet进行请求逻辑处理，使用jsp进行网页展现。
    Jsp的全局代码块：
        特点：
            声明的jva代码作为全局代码转义到对应的servlet类中
        使用：
            <%! 全局代码 %>
        注意：
            全局代码声明，需要在局部代码中调用
    Jsp的脚本段语句：
        特点：
            帮我快速的获取变量或者方法的返回值作为数据响应给浏览器
            起到动态的效果，在网页中显示变量
        使用：
            <%=变量名或者方法%>
        转义：
            out.print(变量名或者方法);
        位置：
            除jsp语法要求意外的任意位置
    Jsp的静态引入和动态引入：
        静态引入：
            使用：
                <%@include file="要引入文件的相对路径" %>
            特点:
                将引入的jsp文件和当前jsp文件转移成一个java使用
                在网页中就显示了合并后的效果
            转义：
                直接将另一个文件的代码复制过来
            注意:
                静态引入的jsp文件不会单独转移成java(servlet)文件。
                不要在多个文件中重复声明同一个变量，会报错。
        动态引入：
            使用：
                <jsp:include page="includeAct.jsp"></jsp:include>
            转义：
                org.apache.jasper.runtime.JspRuntimeLibraryu.include(request,response,"includeAct.jsp",out.false);
            特点：
                会将引入的jsp文件单独转义，在当前文件转义好的java文件中调用引入的jsp文件
                在网页中显示合并后的效果
            注意：
                动态引入可以有重名变量。
        优点：
            降低jsp代码的冗余，便于维护省级。
    Jsp的转发标签forward：
        使用：
            <jsp:forward page="forward.jsp"></jsp:forward>
        特点：
            一次请求，地址栏信息不改变
        注意：
            转发标签的两个标签中间除了写<jsp:param name="str" value="aaa"/>不会报错，其他任意的都会报错
            会将数据以？的形式拼接在转发路径的后面
        取数据：
            <%=request.getParameter("str")%>
    *Jsp的九大内置对象：
        内置对象：
            jsp文件在转义成其对应的Servlet文件的时候自动生成的并声明的对象。我们在jsp页面中直接使用即可。
        注意：
            内置对象在jsp中使用，使用局部代码块或者脚本段诗句来使用，不能在全局代码块中使用。
        内容：
            pageContext 页面上下文对象，其中封存了另外八个对象，当前jsp的运行信息。
                类型：PageContext
                注意：每个jsp文件单独拥有一个pageContext对象
                作用域：_JspService方法体内，当前页面。
            *request 封存当前请求数据的对象。由tomcat服务器创建。
                类型：HttpServletRequest
                作用域：一次请求
            *session 用来存储用户的不同请求的共享数据。
                类型：HttpSession
                作用域：一次会话
            *application ServletContext实现类对象，一个项目只有一个。存储用户共享数据的对象，以及完成其他操作。
                类型：ServletContext
                作用域：项目内
            *response 响应对象，用来响应请求处理结果，可以设置响应头和重定向
                类型：HttpServletResponse
            out 响应对象，Jsp内部使用，带有缓冲区的响应对象，效率高于response对象。
                类型：JspWriter
            page 代表当前Jsp的对象。相对于java中的this。
                类型：Object
            exception 异常对象，存储了当前运行的异常信息。
                类型： Throwable
                注意： 使用此对象需要在page指定中使用isErrorPage="true"开启
            config ServletConfig，主要是用来获取web.xml中的配置数据，完成一些初始化数据的读取。
                类型： ServletConfig
    *四个作用域对象：
        PageContext 当前jsp，当前页面。解决了在当前页面内的数据共享问题
            获取其他内置对象
        request： 一次请求。一次请求的servlet的数据共享
            通过请求转发，将数据流转给下一个servlet
        session： 一次会话。一个用户的不同请求的数据共享
            将数据从同一个用户的一次请求流转给其他请求
        application： 项目内。不同用户的数据共享问题
            将数据从一个用户流转给其他用户
        作用：
            数据流转
    jsp的路径：
        jsp中资源路径可以使用相对路径跳转
            相对路径的话，文件的位置不能随意乱改。
            需要使用../进行文件夹的跳出，使用比较麻烦
        使用绝对路径（必会 建议）
            使用地址的绝对路径`/jsp/forward.jsp` 第一个/代表服务器根目录
            /虚拟项目名/资源名
        myEclipse使用jsp中自带的全局路径声明
            使用: String path = request.getContextPath();
                String basePath = request.getScheme()+"+//" + request.getServerName() +":" + request.getServerPort() + path;
                <base href = "<%=basePath%>">
            作用：
                所有的地址默认自带`localhost:1080/虚拟项目名/`
--%>

<html>
    <head>
        <title>jsp基本语法学习</title>
        <meta charset="utf-8">
    </head>
    <body>
        <h3>jsp基本语法学习</h3>
        <hr />
        <%-- 如果a>3 就显示输出下面这句话 --%>
        <%
            // 声明java局部代码域
            int a = 2;
            String str = "jsp的脚本段语句";
            if(a>3){
        %>
        <b>jsp学习很简单</b>
        <%
            }else{
                test();
        %>
        <i><%=str%></i>
        <%
            }
        %>

        <%-- 全局代码块 直接声明在类下，_JspService方法外 --%>
        <%!
            int b = 5;
            public void test(){
                System.out.println("我是全局代码块");
            }
        %>

        <%-- jsp的静态引入 --%>
        <%@include file="include.jsp"%>
        <%-- jsp的动态引入 --%>
        <jsp:include page="includeAct.jsp"></jsp:include>
        <%-- jsp的转发forward标签--%>
        <jsp:forward page="forward.jsp">
            <jsp:param name="str" value="aaa"/>
        </jsp:forward>

        <%-- jps的九大内置对象--%>
        <%
            // 获取请求数据
            String s = request.getParameter("str");
            request.getAttribute("str");
        %>
        <%
            response.sendRedirect("forward.jsp");
        %>

        <%-- jsp的路径 --%>
        <a href="forward.jsp">forward.jsp</a>

    </body>
</html>

```

### 3.1.10. @WebServlet属性列表：

- 属性名	类型	描述
- name	String	指定Servlet 的 name 属性，等价于 <servlet-name>。如果没有显式指定，则该 Servlet 的取值即为类的全限定名。
- value	String[]	该属性等价于 urlPatterns 属性。两个属性不能同时使用。
- urlPatterns	String[]	指定一组 Servlet 的 URL 匹配模式。等价于<url-pattern>标签。
- loadOnStartup	int	指定 Servlet 的加载顺序，等价于 <load-on-startup>标签。
- initParams	WebInitParam[]	指定一组 Servlet 初始化参数，等价于<init-param>标签。
- asyncSupported	boolean	声明 Servlet 是否支持异步操作模式，等价于<async-supported> 标签。
- description	String	该 Servlet 的描述信息，等价于 <description>标签。
- displayName	String	该 Servlet 的显示名，通常配合工具使用，等价于 <display-name>标签。

### 3.1.11. 信息管理系统项目实战
【ManagerSystem】
技术需求：
Servlet+jsp+mvc+jdbc

软件需求：
开发工具：idea
数据库：mysql
服务器：tomcat
浏览器：chrome

硬件需求：
一台电脑。

功能需求：
  - 完成用户登录
  - 完成用户注册
  - 完成用户退出
  - 完成查看个人信息
  - 完成修改密码
  - 完成查询所有用户信息

数据库设计：
  - 创建用户表
  - 表明t_user
  - 表设计

| 字段名   | 类型          | 约束       |
|-------|-------------|----------|
| uid   | int(10)     | 主键、非空、自增 |
| uname | varchar(50) | 非空       |
| pwd   | varchar(50) | 非空       |
| sex   | char(2)     | 非空       |
| age   | int(3)      |          |
| birth | date        |          |

代码规范：
- 命名规范
    - 包名：com.wyj.*
    - 类名：首字母大写
    - 变量名和方法名：驼峰原则
- 注释规范
    - 方法功能注释
    - 方法体核心位置必须有说明注释
- 日志
    - 可以使用log4j进行日志输出（可选）
    - 数据流转的位置必须有后台输出语句。

功能设计：
- 用户登录
    - 创建登录页面

## 3.2. ajax的概念和原理
### 3.2.1. 什么是ajax
异步刷新技术，用来在当前页面内响应不同请求的内容
异步JavaScript和xml。
本质上是一个浏览器端的技术。不是一种新技术，是如下集中技术的组合应用：
- 基于web标准的XHTML+CSS的表示
- 使用DOM进行动态显示及交互
- 使用XML和XSLT进行数据交换及相关操作
- 使用XMLHttpRequest进行异步数据查询、检索
- 使用JavaScript将所有的东西绑定在一起
### 3.2.2. 为什么需要ajax
需求：
    需要将本次响应结果和前面的响应结果内容在同一个页面中展现给用户
解决：
    1. 在后台服务器端将多次响应内容重新拼接成一个jsp页面，响应。 但是会造成内容的重复响应，资源浪费
    2. 使用ajax技术
### 3.2.3. 使用ajax
#### 3.2.3.1. ajax访问原理
js函数中的ajax引擎对象通过ajax访问引擎(XMLHttpRequest)发送请求给服务器，服务器响应，在当前页面显示。与传统的访问方式对于服务器来说一样处理，对于浏览器来说不同，传统方法要刷新页面，而对于ajax不能刷新页面。
#### 3.2.3.2. ajax的基本使用流程
- 创建ajax引擎
- 复写onreadystatement
    - 判断ajax状态码
        - 判断响应状态码
    - 获取响应内容（相应内容的格式）
    - 处理
- 发送请求
```
<script type="text/javascript">
        function getData(){
            // 创建ajax引擎对象
            var ajax;
            if(window.XMLHttpRequest){ //新版本的浏览器都可以 火狐
                ajax = new XMLHttpRequest();
            }else if(window.ActiveXObject){// ie
                ajax = new ActiveXObject("Msxml2.XMLHTTP")
            }
            // 复写onreadystatement函数
            ajax.onreadystatechange=function(){
                // 获取响应内容
                var result = ajax.responseText;
                // 获取元素对象
                var showdiv = document.getElementById("showdiv");
                // 修改元素内容
                showdiv.innerHTML=result;

            }
            // 发送请求 请求方式和请求地址
            ajax.open("get", "ajax");
            ajax.send(null);
        }
    </script>
```
#### 3.2.3.3. ajax的状态码
ajax.readyState值：
- 0 表示XMLHttpRequest已建立，但还未初始化，这时尚未调用open方法
- 1 表示open方法已调用，但未调用send发放
- 2 表示send方法已调用，其他数据未知
- 3 表示请求已经成功发送，正在接受数据
- 4 表示数据已经接受成功

状态码改变一次，ajax.onreadystatechange函数执行一次。所以总共执行4次，0的时候还未声明，所以不执行。

响应状态码 ajax.status
- 200 成功
- 404
- 500

#### 3.2.3.4. ajax的异步和同步
```
// 发送请求 请求方式和请求地址 第三个参数设置异步和同步默认true异步
            ajax.open("get", "ajax", false);
            ajax.send(null);
            alert("哈哈")
```
同步的话，必须send执行完毕再执行alert。
异步的话，就继续往下执行，另外在等资源来在显示。
#### 3.2.3.5. ajax的请求
```
//get
        // ajax.open("get","ajax?"+data);
        // ajax.send(null);
        //post
        ajax.open("post","ajax");
            // 如果是post要设置数据方式，是键值对的形式，默认是字节码（用于电影图片）
        ajax.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
        ajax.send("name=张三&pwd=123");
```

#### 3.2.3.6. ajax的响应数据类型
- 普通字符串
- json（重点）
- xml

```
// 设置请求编码格式
        req.setCharacterEncoding("utf-8");
        // 设置响应编码格式
        resp.setContentType("text/html;charset=utf-8");
        // 获取请求信息
        String name = req.getParameter("name");
        System.out.println(name);
        // 处理请求信息
        UserService us = new UserServiceImp();
        User u = us.getUserInfoService(name);
        // 响应请求信息
        resp.getWriter().write(new Gson().toJson(u));
```

```
 <script type="text/javascript">
        // 获取数据
            function getData(){
                // 获取用户请求数据
                var name = document.getElementById("name").value;
                //创建ajax
                var ajax;
                if(window.XMLHttpRequest){
                    ajax = new XMLHttpRequest();
                }else if(window.ActiveXObject){
                    ajax = new ActiveXObject("Msml2.XMLHTTP");
                }
                // 复写onreadystatement
                ajax.onreadystatechange=function () {
                    if(ajax.readyState==4){
                        if(ajax.status==200){
                            var result = ajax.responseText;
                            // 将java过来的代码由var变成代码
                            eval("var u=" + result);
                            // 处理相应数据
                                //获取table对象
                                var ta=document.getElementById("ta");
                                ta.innerHTML="";
                            var tr = ta.insertRow(0);
                            var cell0 = tr.insertCell(0);
                            cell0.innerHTML="编号";
                            var cell1 = tr.insertCell(1);
                            cell1.innerHTML="名称";
                            var cell2 = tr.insertCell(2);
                            cell2.innerHTML="价格";
                            var cell3 = tr.insertCell(3);
                            cell3.innerHTML="位置";
                            var cell4 = tr.insertCell(4);
                            cell4.innerHTML="描述";

                                var tr = ta.insertRow(1);
                                var cell0 = tr.insertCell(0);
                                cell0.innerHTML=u.uid;
                            var cell1 = tr.insertCell(1);
                            cell1.innerHTML=u.name;
                            var cell2 = tr.insertCell(2);
                            cell2.innerHTML=u.price;
                            var cell3 = tr.insertCell(3);
                            cell3.innerHTML=u.loc;
                            var cell4 = tr.insertCell(4);
                            cell4.innerHTML=u.desc;

                        }
                    }
                }
                // 发送请求
                ajax.open("get", "user?name="+name);
                ajax.send(null);
            }
    </script>
```
#### 3.2.3.7. ajax的封装

## 3.3. 过滤器
需要对服务器的资源进行统一的管理，比如请求编码格式的统一设置，资源的统一分配等。
作用：
- 对资源进统一管理
- 对servlet提供了一层保护，对请求进行拦截

发生在servlet前后

```java
package com.wyj.servlet;

import javax.jws.WebService;
import javax.servlet.*;
import javax.servlet.annotation.WebFilter;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;

/**
 * 过滤器
 * 作用：
 *      对服务器接收的请求资源和响应给浏览器的资源进行管理。
 *      保护servlet
 * 使用：
 *      创建一个实现了filter接口的普通java类
 *      复写接口方法
 *          init：服务器启动就执行。资源初始化
 *          doFilter：拦截请求的方法，需要在此方法中对资源实现管理。
 *                  注意：
 *                      需要手动对请求进行放行。filterChain.doFilter(servletRequest, servletResponse);
 *          destory：服务器关闭的时候执行
 *      在web.xml中配置过滤器
 *          /* 拦截所有请求
 *          *.do 以.do结尾的请求，一般进行模块拦截处理。
 *          具体名字 拦截具体的某一个请求
 *
 * 生命周期：
 *      服务器启动到服务器关闭
 *
 * 总结：
 *      过滤器程序员声明和配置，服务器根据请求中的uri信息调用。
 * 执行：
 *      浏览器发起请求到服务器，服务器接收到请求后，根据uri信息在web.xml中找到对应的
 *      过滤器执行doFilter方法，该方法对此次请求进行处理后如果符合要求作为放行，放行后
 *      如果还有符合要求的过滤则继续进行过滤，找到执行对应的servlet进行请求处理。servlet对
 *      请求处理完毕后，也就service方法结束了。还需继续返回响应的doFilter方法继续执行。
 *
 * 案例：
 *      统一编码格式设置。
 *      session管理。判断session是否失效
 *      权限管理
 *      资源管理（统一水印，和谐词汇等等）
 */

@WebFilter(value = "/*")
public class MyFilter implements Filter {

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("我被初始化了");
    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("我被doFilter了");
        // 设置编码格式
        servletRequest.setCharacterEncoding("utf-8");
        servletResponse.setContentType("text/html;charset=utf-8");
        // 判断session
        HttpSession hs = ((HttpServletRequest)servletRequest).getSession();
        if(hs.getAttribute("user")==null){
            ((HttpServletResponse)servletResponse).sendRedirect("/a/login.jsp");
        }else {
            // 放行 去执行servlet
            filterChain.doFilter(servletRequest, servletResponse);
        }
        System.out.println("我被doFilter了2");
    }

    @Override
    public void destroy() {
        System.out.println("我被destroy了");
    }
}

```

## 3.4. 监听器
request、session、application作用域对象，其作用是实现数据的在不同场景中的灵活流转。但是数据的具体流转过程我面试看不到的，比如作用域对象是什么时候创建和销毁的，数据是什么时候存取，改变和删除的。因为具体的流转过程看不到，所以就无法再指定的时机对数据和对象进行操作。比如session销毁的时候，在线人数-1。

发生在request、session、application前后

### 3.4.1. 监听对象
request session application

### 3.4.2. 监听内容
创建、销毁、属性改变事件

### 3.4.3. 监听作用
在事件发生之前，之后进行一些处理

```java
import javax.servlet.ServletContext;
import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;
import javax.servlet.annotation.WebListener;
import javax.servlet.http.HttpSessionAttributeListener;
import javax.servlet.http.HttpSessionEvent;
import javax.servlet.http.HttpSessionListener;
import javax.servlet.http.HttpSessionBindingEvent;

@WebListener
public class MyListener implements ServletContextListener,
        HttpSessionListener, HttpSessionAttributeListener {

    // Public constructor is required by servlet spec
    public MyListener() {
    }

    // -------------------------------------------------------
    // ServletContextListener implementation
    // -------------------------------------------------------
    public void contextInitialized(ServletContextEvent sce) {
      /* This method is called when the servlet context is
         initialized(when the Web application is deployed). 
         You can initialize servlet context related data here.
      */
      // 使用application
      ServletContext sc = sce.getServletContext();

      // 在application对象存储变量用来统计在线人数
        sc.setAttribute("count", 0);
    }

    public void contextDestroyed(ServletContextEvent sce) {
      /* This method is invoked when the Servlet Context 
         (the Web application) is undeployed or 
         Application Server shuts down.
      */
    }

    // -------------------------------------------------------
    // HttpSessionListener implementation
    // -------------------------------------------------------
    public void sessionCreated(HttpSessionEvent se) {
        /* Session is created. */
        // 获取servletcontext对象
        ServletContext sc = se.getSession().getServletContext();
        // 获取在线统计人数的变量
        int count = (int) sc.getAttribute("count");
        sc.setAttribute("count", ++count);
    }

    public void sessionDestroyed(HttpSessionEvent se) {
        /* Session is destroyed. */
        ServletContext sc = se.getSession().getServletContext();
        // 获取在线统计人数的变量
        int count = (int) sc.getAttribute("count");
        sc.setAttribute("count", --count);
    }

    // -------------------------------------------------------
    // HttpSessionAttributeListener implementation
    // -------------------------------------------------------

    public void attributeAdded(HttpSessionBindingEvent sbe) {
      /* This method is called when an attribute 
         is added to a session.
      */
    }

    public void attributeRemoved(HttpSessionBindingEvent sbe) {
      /* This method is called when an attribute
         is removed from a session.
      */
    }

    public void attributeReplaced(HttpSessionBindingEvent sbe) {
      /* This method is invoked when an attribute
         is replaced in a session.
      */
    }
}
```

---
