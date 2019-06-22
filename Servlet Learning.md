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
