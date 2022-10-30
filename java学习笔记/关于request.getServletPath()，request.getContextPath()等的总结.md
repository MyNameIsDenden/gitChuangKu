## 关于request.getServletPath()，request.getContextPath()等的总结

最近对于request中的几种“路径”有点混淆，查找网上资源都没有很好的总结，希望此文章能够帮助我理解一下这几种“路径”。
+++++++++++++++++++++++++++++++++++++++++++++++++
本文章主要讨论以下几种request获取路径的方法：

```
request.getServletPath()
request.getPathInfo()
request.getContextPath()
request.getRequestURI()
request.getRequestURL()
request.getServletContext().getRealPath()
```

以一个简单的例子说明：
web.xml配置（注意此处的url-pattern项）

```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" id="WebApp_ID" version="2.5">
  <display-name>aab</display-name>
  <welcome-file-list>
    <welcome-file>a.jsp</welcome-file>
  </welcome-file-list>
	<servlet>
		<servlet-name>test</servlet-name>
		<servlet-class>com.java.test.TestServlet</servlet-class>
		

	</servlet>
	<servlet-mapping>
		<servlet-name>test</servlet-name>
		<url-pattern>/*</url-pattern><!-- 注意此处 -->
	</servlet-mapping>

</web-app>
```

TestServlet.java文件：

```
package com.java.test;

import java.io.IOException;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class TestServlet extends HttpServlet{
	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		this.doPost(req, resp);
	}
	@Override
	protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		System.out.println("servletPath:"+req.getServletPath());
		System.out.println("contextPath:"+req.getContextPath());
		System.out.println("contextPath2:"+req.getServletContext().getContextPath());
		System.out.println("pageInfo:"+req.getPathInfo());
		System.out.println("uri:"+req.getRequestURI());
		System.out.println("url:"+req.getRequestURL());
		System.out.println("realPath:"+req.getServletContext().getRealPath("/"));

	}

}
```

此时请求http://localhost:8080/testweb (url-pattern=/*)
打印出来的值为：

```
servletPath:
contextPath:/testweb
contextPath2:/testweb
pageInfo:null
uri:/testweb
url:http://localhost:8080/testweb
realPath:G:\java\.metadata\.plugins\org.eclipse.wst.server.core\tmp0\wtpwebapps\testweb\
```

请求http://localhost:8080/testweb/abc 打印的值为：

```
servletPath:
contextPath:/testweb
contextPath2:/testweb
pageInfo:/abc
uri:/testweb/abc
url:http://localhost:8080/testweb/abc
realPath:G:\java\.metadata\.plugins\org.eclipse.wst.server.core\tmp0\wtpwebapps\testweb\
```

当我们修改web.xml为如下时（注意url-pattern的改变）：

```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" id="WebApp_ID" version="2.5">
  <display-name>aab</display-name>
  <welcome-file-list>
    <welcome-file>a.jsp</welcome-file>
  </welcome-file-list>


	<servlet>
		<servlet-name>test</servlet-name>
		<servlet-class>com.java.test.TestServlet</servlet-class>
		
	</servlet>
	
	<servlet-mapping>
		<servlet-name>test</servlet-name>
		<url-pattern>/abc/def/*</url-pattern><!-- 注意此处 -->
	</servlet-mapping>

</web-app>
```


请求http://localhost:8080/testweb/abc/def/ghi/test.html (url-pattern=/abc/def/*)
打印的值为：

```
servletPath:/abc/def
contextPath:/testweb
contextPath2:/testweb
pageInfo:/ghi/test.html
uri:/testweb/abc/def/ghi/test.html
url:http://localhost:8080/testweb/abc/def/ghi/test.html
realPath:G:\java\.metadata\.plugins\org.eclipse.wst.server.core\tmp0\wtpwebapps\testweb\
```


通过观察打印结果，我们可以总结：

getServletPath():获取能够与“url-pattern”中匹配的路径，注意是完全匹配的部分，*的部分不包括。
getPathInfo():与getServletPath()获取的路径互补，能够得到的是“url-pattern”中*d的路径部分
getContextPath():获取项目的根路径
getRequestURI:获取根路径到地址结尾
getRequestURL:获取请求的地址链接（浏览器中输入的地址）
getServletContext().getRealPath("/"):获取“/”在机器中的实际地址
getScheme():获取的是使用的协议(http 或https)
getProtocol():获取的是协议的名称(HTTP/1.11)
getServerName():获取的是域名(xxx.com)
getLocalName:获取到的是IP