## 十三、 Web编程

#### 91. 什么是Servlet？

Servlet是基于Java语言的web服务器端编程技术，是用来处理客户端请求并产生动态网页内容的Java类。

Servlet主要是用来处理或者是存储HTML表单提交的数据，产生动态内容，在无状态的HTTP协议下管理状态信息。

#### 92. 说一下Servlet的体系结构。

所有的Servlet都必须要实现的核心的接口是javax.servlet.Servlet。每一个Servlet都必须要直接或者是间接实现这个接口，或者是继承javax.servlet.GenericServlet或者javax.servlet.http.HTTPServlet。最后，Servlet使用多线程可以并行的为多个请求服务。

GenericServlet抽象类实现了javax.servlet.Servle接口，给出了设计Servlet的一些骨架，定义了Servlet生命周期和一些得到名字、配置、初始化参数的方法，其设计是和应用层协议无关的。而javax.servlet.http.HttpServlet是javax.servlet.GenericServlet的子类，定义了与Http协议相关的doGet/doPost方法。

#### 93. Applet和Servlet有什么区别？

Applet是运行在客户端主机的浏览器上的客户端Java程序。而Servlet是运行在web服务器上的服务端的组件。

applet可以使用用户界面类，而Servlet没有用户界面，相反，Servlet是等待客户端的HTTP请求，然后为请求产生响应。

#### 97. 什么是Web应用程序？(暂时)

Web应用程序是对Web或者是应用服务器的动态扩展，是一种可以通过Web访问的应用程序

有两种类型的Web应用：面向表现的和面向服务的。

面向表现的Web应用程序会产生包含了很多种标记语言和动态内容的交互的web页面作为对请求的响应。

而面向服务的Web应用实现了Web服务的端点(endpoint)。一般来说，一个Web应用可以看成是一组安装在服务器URL名称空间的特定子集下面的Servlet的集合。

#### 98. 什么是服务端包含(Server Side Include)？

服务端包含(SSI)是一种简单的解释型服务端脚本语言，大多数时候仅用在Web上，用servlet标签嵌入进来。

SSI最常用的场景把一个或多个文件包含到Web服务器的一个Web页面中。当浏览器访问Web页面的时候，Web服务器会用对应的servlet产生的文本来替换Web页面中的servlet标签。

#### 99. 什么是Servlet链(Servlet Chaining)？

Servlet链是把一个Servlet的输出发送给另一个Servlet的方法。第二个Servlet的输出可以发送给第三个Servlet，依次类推。链条上最后一个Servlet负责把响应发送给客户端。

#### 100. 如何知道是哪一个客户端的机器正在请求你的Servlet？

ServletRequest类可以找出客户端机器的IP地址或者是主机名。

> getRemoteAddr()方法获取客户端主机的IP地址，getRemoteHost()可以获取主机名。

### 101. HTTP响应的结构是怎么样的？

HTTP响应由三个部分组成：状态码、HTTP响应头和HTTP响应体。
> 状态码(Status Code)：描述了响应的状态。可以用来检查是否成功的完成了请求。请求失败的情况下，状态码可用来找出失败的原因。如果Servlet没有返回状态码，默认会返回成功的状态码HttpServletResponse.SC_OK。
> 
> HTTP响应头(HTTP Header)：它们包含了更多关于响应的信息。比如：头部可以指定认为响应过期的过期日期，或者是指定用来给用户安全的传输实体内容的编码格式。
> 
> HTTP响应体(Body)：它包含了响应的内容。它可以包含HTML代码，图片，等等。主体是由传输在HTTP消息中紧跟在头部后面的数据字节组成的。

### 102. 什么是cookie？session和cookie有什么区别？

cookie存储于客户端，session存储于服务端。

cookie实际上是一小段的文本信息。客户端请求服务器，如果服务器需要记录该用户的状态，就使用response向客户端浏览器颁发一个cookie。客户端浏览器会把cookie保存起来。当浏览器再次请求该网站时，浏览器就会把请求地址和cookie一同给服务器。服务器检查该cookie，从而判断用户的状态。服务器还可以根据需要修改cookie的内容。

session是另一种记录客户状态的机制。不同的是cookie保存在客户端浏览器中，而session保存在服务器上。客户端浏览器访问服务器的时候，服务器把客户端信息以某种形式记录在服务器上，这就是session。客户端浏览器再次访问时只需要从该session中查找该客户的状态就可以了。


#### 103. 浏览器和Servlet通信使用的是什么协议？

浏览器和Servlet通信使用的是HTTP协议。

#### 104. 什么是HTTP隧道？

HTTP隧道是一种利用HTTP或者是HTTPS把多种网络协议封装起来进行通信的技术。
因此，HTTP协议扮演了一个打通用于通信的网络协议的管道的包装器的角色，把其他协议的请求掩盖成HTTP的请求就是HTTP隧道。

#### 105. sendRedirect()和forward()方法有什么区别？

redirect是客户端跳转，跳转后页面地址栏会发生改变

> 当浏览器访问地址A时，sendRedirect()方法会让客户端创建一个新的请求去访问另外一个地址B，重定向(redirect)以后，客户端浏览器地址栏中的地址显示的是地址B，因为会产生一个新的请求，之前请求作用域范围以内的对象就失效了，因此这个方式地址A和地址B之间不能共享数据。 

forward是服务器跳转，跳转后页面的地址栏不会改变

> 当浏览器访问地址A时，地址A先处理请求，forward()方法会让地址A去寻找地址B，让地址B接着去处理请求。forward()方法只是把请求转发到一个新的目标上，而转发(forwarding)以后，之前请求作用域范围以内的对象还是能访问的，这个方式浏览器地址栏显示的一直是地址A，两个地址之间能共享数据。

一般认为sendRedirect()比forward()要慢。








Web编程Jsp&Servlet技术
1、简单说说tomcat的配置？
JAVA_HOME=JDK的根目录
CATALINA_HOME=tomcat的根目录

CATALINA-HOME\conf\server.xml:可以配置tomcat的端口，可以配置tomcat中下连接池。
CATALINA-HOME\common\lib:存放公用的类包

在My eclipse中如何配置tomcat
在eclipse中,选择windows->preference->MyEclipse->ApplicationServer->Tomcat
选择Tomcat 的安装目录，并选择TomCat所需的jdk，选择enable,确定即可。
2、JSP中动态INCLUDE与静态INCLUDE的区别？
jsp:include:在运行时调用另一个页面，变量是可以重复的。
<%@include file=””%>:在转译时合在一起，会成为同一个类，变量不可以重复。
3、forward和redirect的区别？
forward: 转发，在下一个页面中,request保留上一个页面中的request的所有值
	
redirect: 跳转，不传递request对象。
4、Servlet的体系结构是什么? 
Servlet
GenericServlet
HttpServlet
自定义
5、如何实现一个自定义的servlet? 
extends HttpServlet 并覆盖doPost或doGet方法

在web.xml中进行部署
6、Servlet的生命周期是什么?
Init 
多次执行doGet或doPost  
destroy
7、jsp就是一个servlet是否正确?
对
8、请罗列jsp中的脚本、指令及动作? 
脚本
			<%%>  <%=%>  <%!%> <%----%>
		指令
			<%@page contentType=”text/html;charset=utf-8” language=”java” import=””%>
			<%@include file=””%>
			<%@taglib uri=”” prefix=””%>
		动作:
			<jsp:useBean class=”” id=”” scope=””>  在scope中如果没有实例化一个对象，如果有直接用以前的。
			<jsp:getProperty name=”” property=””>  向一个bean中设置属性值
			<jsp:forward >  jsp页的转发
			<jsp:include page=””>  导入一个jsp页面
9、JSP的内置对象及方法
Request  request表示HttpServletRequest对象。取客户端表单域信息及cookie, header, 和session 
response response表示HttpServletResponse对象，对客户端的响应返回文本、写cookies。
out out 向客户端打印html文本. 
pageContext :当前jsp页面的上下文环境，可以得到session、request、application等内置对象，在自定义标签中使用的很多。
session session表示一个请求的javax.servlet.http.HttpSession对象。Session一个用户多个页面共享同一变量。
application applicaton 表示一个javax.servle.ServletContext对象。存放容器级的变量。
config config表示一个javax.servlet.ServletConfig对象。该对象用于存取servlet实例的初始化参数。 
page page表示从该页面产生的一个servlet实例
exception:异常，当iserrorpage=true
10、说出在JSP页面里是怎么分页的?
页面需要保存以下参数：(数据库的分页及比较)
总行数：根据sql语句得到总行数
每页显示行数：设定值
当前页数：请求参数
页面根据当前页数和每页行数计算出当前页第一行行数，定位结果集到此行，对结果集取出每页显示行数的行即可。

数据库:
Sqlserver:
		SELECT TOP 页大小 *
FROM TestTable

WHERE (ID NOT IN
        (SELECT TOP 页大小*(页数-1) id
         FROM 表
         ORDER BY id))
ORDER BY ID


--pageSize=5  页大小
--pageIndex=2 所要的页
--如果有主键可以，没以及键不行
select top 5 * 
from aa where a1 not in
(select top 5 a1 from aa order by a1)
order by a1;

oracle:
	select * from '||v_sql||' where rownum<'||
         pageLastRow||'minus select * from '||v_sql
         ||' where rownum<'||pageFirstRow;


Session
	先取出数据中的所有信息封装到对象并保存在session中，转发到jsp页面做如下处理。
	<table border="1">
    <tr>
      <td>a1</td>
      <td>a2</td>
    </tr>
<%
     List l=(List)session.getAttribute("as");
     //一页显示多少行
     int pageSize=3;
     //总页数
     int pageCount=0;
     int currentPage=1;
     if(l!=null && l.size()>0)
     {
     pageCount=(l.size()/pageSize)+(l.size()%pageSize==0?0:1);
     if(request.getParameter("page")!=null)
     {
       currentPage=Integer.parseInt(request.getParameter("page"));
     }
     if(currentPage<1)
     {
       currentPage=1;
     }
     if(currentPage>pageCount)
     {
       currentPage=pageCount;
     }
     for (int i = (currentPage-1)*pageSize; i <(currentPage-1)*pageSize+pageSize; i++)
     {
       if(i>=l.size())
       {
         break;
       }
       Aa aa=(Aa)l.get(i);
       %>
        <tr>
      <td><%=aa.getA1()%></td>
      <td><%=aa.getA2()%></td>
    </tr>
       <%
       }
     }
%>
<tr>
  <td colspan="2">
    <%
    if(currentPage!=1)
    {
    %>
    <a href="page.jsp?page=1">首页</a>&nbsp;&nbsp;
    <a href="page.jsp?page=<%=currentPage-1%>">上一页</a>&nbsp;&nbsp;
    <%
    }
    if(currentPage!=pageCount)
    {
    %>
    <a href="page.jsp?page=<%=currentPage+1%>">下一页</a>&nbsp;&nbsp;
    <a href="page.jsp?page=<%=pageCount%>">最后一页</a>&nbsp;&nbsp;
    <%
    }
    %>
  </td>
</tr>
  </table>
	
11、include的两种实现方式的区别?
<@include file>:在将jsp生成servlet类前将两个文件和在一起，生成一个java类，一起运行的。所以是一家子，当中的变量名不能重名。
<jsp:include page>:是两个类，是一个调用关系，在运行时动态的调用，不是一家子，可以重复变量。
12、jsp页面中两种跳转方式分别是什么?有什么区别?
转发: 保留上次的request
		<jsp:forward>
		actionMapping.findForWard(“”);
		pageContext.forward();
		request.getRequestDispacher(“a.jsp”).forward(request,response)
跳转:不保留上次的request
		Response.setRedirect(“”)
13、描述JSP和Servlet的区别、共同点、各自应用的范围
Jsp主要在于页面的显示动态生成页面，可以与html标记一起使用，其还是要生成为一个servlet。
Servlet：主要是控制的处理，如调用业务层，跳转不同的jsp页面。
Mvc:
		Jsp:v
		Servlet:c
14、在JSP中如何读取客户端的请求，如何确定某个Jsp文件的真实路径？
Request.getparameter(“”)
<%=application.getRealPath("aa.jsp") %>
15、描述Cookie和Session的作用，区别和各自的应用范围，Session工作原理。
Cookie:主要用在保存客户端，其值在客户端与服务端之间传送，不安全，存储的数据量有限。
Session:保存在服务端，每一个session在服务端有一个sessionID作一个标识。存储的数据量大，安全性高。占用服务端的内存资源。
16、说明Jsp中errorPage的作用，应用范围。
正常页面中
%@page erropage=”error.jsp”%
		错误页面
			<%@page iserrorpage=”true”%>
				 有一内置对象:exception
17、介绍在Jsp中如何使用JavaBeans
<jsp:useBean class=”” id=”” scope=””/>
<%
	New 类();
%>
19、简单介绍JSP的标记库
做一个标记处理类 extends TagSupport
通过tld说明标记处理的类的前缀及后缀
在web.xml中说明tld文件 
<taglib>
		<taglib-uri>
		<taglib-location>
<taglib>
在jsp页面是引用tld<%@taglib uri=”” prefix=””%>
20、Servlet中的核心类有那些，各有什么特点?
ServletContext：容器，放置全局变量
	setAtribute()
	getAttribute()
ServletConfig：一个servlet的配置
	getInitParameter(”名称”)
HttpServletRequest：封装的所有的请求
	getParameterValue(”名称”)
	getParameterValues(”称”)
getSession();
	getAttribute(” 名称”);
	getRequestDispatch(”a.jsp”).forward(request,response)
HttpServletResponse：响应
	getOut();
	sendRedirect(””)							
HttpSession：一个用户多个页面共享同一变量
	setAttribute(””,””)
21、Servlet中重要的包有那些，有什么区别?
javax.servlet.*；javax.servlet.http.*;
22、说出Servlet的生命周期，并说出Servlet和CGI的区别？
Servlet被服务器实例化后，容器运行其init方法，请求到达时运行其service方法，service方法自动派遣运行与请求对应的doXXX方法（doGet，doPost）等，当服务器决定将实例销毁的时候调用其destroy方法。

与cgi的区别在于servlet处理服务器进程中，它通过多线程方式运行其service方法，一个实例可以服务于多个请求，并且其实例一般不会销毁，而CGI对每个请求都产生新的进程，服务完成后就销毁，所以效率上低于servlet。
23、什么情况下调用doGet()和doPost()？
Jsp页面中的form标签里的method属性为get时调用doGet()，为post时调用doPost()。 
25、如何现实servlet的单线程模式
在doGet及doPost方法前加入synchoronized
JSP:
<%@ page isThreadSafe="true"%>
27、Request对象的主要方法：
setAttribute(String name,Object)：设置名字为name的request的参数值
getAttribute(String name)：返回由name指定的属性值
getAttributeNames()：返回request对象所有属性的名字集合，结果是一个枚举的实例
getCookies()：返回客户端的所有Cookie对象，结果是一个Cookie数组
getCharacterEncoding()：返回请求中的字符编码方式
getContentLength()：返回请求的Body的长度
实例
getInputStream()：返回请求的输入流，用于获得请求中的数据
getMethod()：获得客户端向服务器端传送数据的方法
getParameter(String name)：获得客户端传送给服务器端的有name指定的参数值
getParameterNames()：获得客户端传送给服务器端的所有参数的名字，结果是一个枚举的实例
getParameterValues(String name)：获得有name指定的参数的所有值
getProtocol()：获取客户端向服务器端传送数据所依据的协议名称
getQueryString()：获得查询字符串
getRequestURI()：获取发出请求字符串的客户端地址
getRemoteAddr()：获取客户端的IP地址
getRemoteHost()：获取客户端的名字
getSession([Boolean create])：返回和请求相关Session
getServerName()：获取服务器的名字
getServletPath()：获取客户端所请求的脚本文件的路径
getServerPort()：获取服务器的端口号
removeAttribute(String name)：删除请求中的一个属性
28、我们在web应用开发过程中经常遇到输出某种编码的字符，如iso8859-1等，如何输出一个某种编码的字符串？
  Public String translate (String str) {
    String tempStr = "";
    try {
      tempStr = new String(str.getBytes("ISO-8859-1"), "GBK");
      tempStr = tempStr.trim();
    }
    catch (Exception e) {
      System.err.println(e.getMessage());
    }
    return tempStr;
  }

30、Servlet执行时一般实现哪几个方法?
public void init(ServletConfig config)
public ServletConfig getServletConfig()
public String getServletInfo()
public void service(ServletRequest request,ServletResponse response)
public void destroy()


### JSP内置对象

https://www.nowcoder.com/profile/7404313/test/7958772/15276?onlyWrong=0

### JSP作用域

https://www.nowcoder.com/profile/7404313/test/7977669/55372?onlyWrong=0
