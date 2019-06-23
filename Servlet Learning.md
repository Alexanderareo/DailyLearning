### Servlet 学习
##### What is servlet
> 运行在Web服务器上或应用服务器上的程序，作为Web浏览器或者其他HTTP客户端的请求和HTTP服务器上的数据库或应用程序之间的中间层
> 
##### Task of servlet
* 读取HTML表单
* HTTP请求数据
* 处理数据，包括对数据库的访问，调用Web服务
* 发送显式数据给客户端，包括文本文件，GIF图像
* 发送隐式HTTP响应到客户端，包括返回的文档类型以及设置Cookie和缓存参数

##### Package of servlet
* Javax.servlet & javax.servlet.http
* 一般采用继承(extends) HttpServlet类的方式

##### Methods of servlet
* init()
* service()
> 每次服务器接收到一个 Servlet 请求时，服务器会产生一个新的线程并调用服务。service() 方法检查 HTTP 请求类型（GET、POST、PUT、DELETE 等），并在适当的时候调用 doGet、doPost、doPut，doDelete 等方法 
* destroy()

##### Deployment of servlet
* Two methods
> > 1、通过 @WebServlet(" ")中写明
> > 2、通过 web.xml来进行设置
```xml
<web-app>      
    <servlet>
        <servlet-name>HelloWorld</servlet-name>
        <servlet-class>HelloWorld</servlet-class>
    </servlet>

    <servlet-mapping>
        <servlet-name>HelloWorld</servlet-name>
        <url-pattern>/HelloWorld</url-pattern>
    </servlet-mapping>
</web-app>
```

##### Forms in Servlet
* Two Methods to Get information
> GET
> > GET 方法是默认的从浏览器向 Web 服务器传递信息的方法，它会产生一个很长的字符串，出现在浏览器的地址栏中.。
> > Servlet 使用 doGet() 方法处理这种类
> >
> POST
> > POST 方法打包信息的方式与 GET 方法基本相同，但是 POST 方法不是把信息作为 URL 中 ? 字符后的文本字符串进行发送，而是把这些信息作为一个单独的消息。消息以标准输出的形式传到后台程序，您可以解析和使用这些标准输出。
> > Servlet 使用 doPost() 方法处理这种类型的请求。

##### Use Servlet to read Form data
* getParameter()：您可以调用 request.getParameter() 方法来获取表单参数的值。
* getParameterValues()：如果参数出现一次以上，则调用该方法，并返回多个值，例如复选框。
* getParameterNames()：如果您想要得到当前请求中的所有参数的完整列表，则调用该方法。

> 在返回页面的时候可以调用response.getWriter()所声明的对象，例如out， out.println();

> <sevrlet-class>需要在方法前带上包名，这里遵循的是类之间的相对路径关系，同时，可以通过Mark category as root来设置CLASSPATH

> 关于返回的response的设置，先前说过，我们可以返回一个html文件，可以使用
```java
response.setContentType("text/html;charset=UTF-8");
```
> 针对复选框的内容,这个html是手打的，会比较丑
```java
PrintWriter out = response.getWriter();
        String title = "读取复选框数据";
        String docType = "<!DOCTYPE html> \n";
            out.println(docType +
                "<html>\n" +
                "<head><title>" + title + "</title></head>\n" +
                "<body bgcolor=\"#f0f0f0\">\n" +
                "<h1 align=\"center\">" + title + "</h1>\n" +
                "<ul>\n" +
                "  <li><b>菜鸟按教程标识：</b>: "
                + request.getParameter("runoob") + "\n" +
                "  <li><b>Google 标识：</b>: "
                + request.getParameter("google") + "\n" +
                "  <li><b>淘宝标识：</b>: "
                + request.getParameter("taobao") + "\n" +
                "</ul>\n" +
                "</body></html>");
```

> 对于getParameterNames所产生的对象(Enumeration类型)，可用的方法有nextElement获取下一个元素

##### HTTP Request From Servlet Client 
* 一些可用的方法
> getAttributeName()
> getParameterNames()
> getSession() 返回session会话，有趣的feature可以研究一下
> getContentType()
> getContextPath() 获取上下文请求的URL部分
> getmethod()
> 

##### Servlet Server Http Response
* 一些对应的Set方法
> 设定时间
```java
import java.io.IOException;
import java.io.PrintWriter;
import java.text.SimpleDateFormat;
import java.util.Calendar;
import java.util.Date;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/Refresh")

//扩展 HttpServlet 类
public class Refresh extends HttpServlet {

    // 处理 GET 方法请求的方法
      public void doGet(HttpServletRequest request,
                        HttpServletResponse response)
                throws ServletException, IOException
      {
          // 设置刷新自动加载时间为 5 秒
          response.setIntHeader("Refresh", 5);
          // 设置响应内容类型
          response.setContentType("text/html;charset=UTF-8");
         
          //使用默认时区和语言环境获得一个日历  
          Calendar cale = Calendar.getInstance();  
          //将Calendar类型转换成Date类型  
          Date tasktime=cale.getTime();  
          //设置日期输出的格式  
          SimpleDateFormat df=new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");  
          //格式化输出  
          String nowTime = df.format(tasktime);
          PrintWriter out = response.getWriter();
          String title = "自动刷新 Header 设置 - 菜鸟教程实例";
          String docType =
          "<!DOCTYPE html>\n";
          out.println(docType +
            "<html>\n" +
            "<head><title>" + title + "</title></head>\n"+
            "<body bgcolor=\"#f0f0f0\">\n" +
            "<h1 align=\"center\">" + title + "</h1>\n" +
            "<p>当前时间是：" + nowTime + "</p>\n");
      }
      // 处理 POST 方法请求的方法
      public void doPost(HttpServletRequest request,
                         HttpServletResponse response)
          throws ServletException, IOException {
         doGet(request, response);
      }
}
```

##### HTTP Status Code
* 设置
```java
public void setStatus ( int statusCode )
public void sendRedirect(String url) //该方法生成一个 302 响应，连同一个带有新文档 URL 的 Location--->Redirect
public void sendError(int code, String message)
```

##### Servlet Filter
* Servlet 过滤器可以动态地拦截请求和响应，以变换或使用包含在请求或响应中的信息。
* 可以将一个或多个 Servlet 过滤器附加到一个 Servlet 或一组 Servlet。Servlet 过滤器也可以附加到 JavaServer Pages (JSP) 文件和 HTML 页面。调用 Servlet 前调用所有附加的 Servlet 过滤器。
> 在客户端的请求访问后端资源之前，拦截这些请求。
> 在服务器的响应发送回客户端之前，处理这些响应。
* 接口 javax.servlet.Filter
> 方法 
> > doFilter(); 该方法完成实际的过滤操作，当客户端请求方法与过滤器设置匹配的URL时，Servlet容器将先调用过滤器的doFilter方法。FilterChain用户访问后续过滤器。
> > init(); web 应用程序启动时，web 服务器将创建Filter 的实例对象，并调用其init方法，读取web.xml配置，完成对象的初始化功能
> > destroy(); 

* 在web.xml中的配置如下
```xml
    <filter>
    <filter-name>LogFilter</filter-name>
    <filter-class>com.runoob.test.LogFilter</filter-class>
    <init-param>
        <param-name>Site</param-name>
        <param-value>菜鸟教程</param-value>
    </init-param>
    </filter>
```
* web.xml配置各节点说明
> <init-param>元素用于为过滤器指定初始化参数，它的子元素<param-name>指定参数的名字，<param-value>指定参数的值。
> 

##### 异常处理
* 在xml文件中<error-page>是关键字
* 如果想对所有的异常有通用的处理程序
```xml
<error-page>
    <exception-type>java.lang.Throwable</exception-type >
    <location>/ErrorHandler</location>
</error-page>
```

##### Servlet and Cookie
* What is cookie？
> Cookie 是存储在客户端计算机上的文本文件，并保留了各种跟踪信息

##### Servlet Session
* Session 用于保持Web客户端和Web服务器间的Session会话

* 一个 Web 服务器可以发送一个隐藏的 HTML 表单字段，以及一个唯一的 session 会话 ID
```xml
<input type="hidden" name="sessionid" value="12345">
```
* URL重写

* HttpSession对象
> Servlet提供了HttpSession接口，Servlet 容器使用这个接口来创建一个 HTTP 客户端和 HTTP 服务器之间的 session 会话。会话持续一个指定的时间段，跨多个连接或页面请求。
```java
HttpSession session = request.getSession();
```
* 同接受的表单信息一样，可以使用getAttribute()和setAttribute()方法
* 添加xml只需要添加与之对应的servlet方法。

##### servlet Database Connection
* 首先需要声明JDBC驱动以及数据库URL
```java
// JDBC 驱动名及数据库 URL
    static final String JDBC_DRIVER = "com.mysql.jdbc.Driver";  
    static final String DB_URL = "jdbc:mysql://localhost:3306/PROJECT_NAME";
    // 数据库的用户名与密码，需要根据自己的设置
    static final String USER = "root";
    static final String PASS = "123456";
    protected void deGet(HttpServletRequest request, HttpServletResponse response)throws ServletException,
    IOException{
        ......
        Connection conn = null;
        Statement stmt = null;
        response.setContentType("text/html;charset=UTF-8");
        PrintWriter out = response.getWriter();
        try{
            Class.forName("com.mysql.jdbc.Driver");
            conn = DriverManager.getConnection(DB_URL,USER,PASS);
            stmt = conn.createStatement();
            String sql;
            sql = "  ";
            ResultSet rs = stmt.executeQurery(sql);
            while(rs.next()){
                int id = rs.getString("name");
                String url = rs.getString("url");
                //这里的getString中的变量是数据库table中的关键字名
            }
            rs.close();
            stmt.close();
            conn.close();
        }catch(SQLException se){
            se.printStackTrace();
        }catch(Exception e){
            e.printStackTrace();
        }finally{
            try{
                if(stmt!=null)
                    stmt.close();
            }catch(SQLException se2){
            }
            try{
                if(conn!=null)
                conn.close();
            }catch(SQLException se){
                se.printStackTrace();
            }
        }
    }
```
> 还有种方法是使用 PreparedStatement

##### Servlet For File Uploading
> Too long to cover.

##### WebPage Redirect
> The most simple method is to use sendRedirect() in response
```java
public void HttpServletResponse.sendRedirect(String location)throws IOException{
}
```
> We can also use setStatus() and setHeader() together
```java
String site = "http://URL" ;
response.setStatus(response.SC_MOVED_TEMPORARILY);
response.setHeader("Location", site); 
```
> Actual Example
```java
@WebServlet("/PageRedirect")
public class PageRedirect extends HttpServlet{
    
  public void doGet(HttpServletRequest request,
                    HttpServletResponse response)
            throws ServletException, IOException
  {
      // 设置响应内容类型
      response.setContentType("text/html;charset=UTF-8");

      // 要重定向的新位置
      String site = new String("http://www.runoob.com");

      response.setStatus(response.SC_MOVED_TEMPORARILY);
      response.setHeader("Location", site);    
    }
} 
```
* 使用注释的WebServlet进行路由绑定的优先级高于web.xml进行绑定


