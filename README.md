# Servlet-JSP 

# Servlet: 
A servlet is a Java program that runs on a web server and is used to handle requests from web clients (like browsers), process them, and send back a response.It helps in creating dynamic web pages

## How Servlet works?
![Servlets_architecture](https://github.com/user-attachments/assets/8be8a486-b1c9-4cfb-af4d-a48d6bc2eec9)

Execution of Servlets basically involves Six basic steps: 

###### 1. The Clients send the request to the Web Server.
###### 2. The Web Server receives the request.
  
  ```
   * Now web server has web container(Apache Tomcat etc). This webcontainer contains a web.xml file (Deployment descriptor) and other servlet classes and html code.
        The server checks if the servlet is requested for the first time.

          If yes, web container does the following tasks:
          
            loads the servlet class.
            instantiates the servlet class.
            calls the init method passing the ServletConfig object
          else
            calls the service method passing request and response objects
  
##### How web container handles the servlet request?
The web container is responsible to handle the request. Let's see how it handles the request.

maps the request with the servlet in the web.xml file.
code example:
/* Start code */ 
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd" id="WebApp_ID" version="4.0">
  <display-name>DemoApp</display-name>
  
 //servlet and servlet-mapping tag have same servlet-name so that it can map each other.
 //this is my -> package: com.aqib.
 //this my servlet class -> AddServerlet
 	<servlet>
 		<servlet-name>abc</servlet-name>
 		<servlet-class>com.aqib.AddServerlet</servlet-class>
 	</servlet>
 	<servlet-mapping>
 		<servlet-name>abc</servlet-name>
 		<url-pattern>/add</url-pattern>
 	</servlet-mapping>
 	
</web-app>
/* end code */

 creates request and response objects for this request
calls the service method on the thread
The public service method internally calls the protected service method
The protected service method calls the doGet or doPost method depending on the type of request.
The doGet method generates the response and it is passed to the client.
After sending the response, the web container deletes the request and response objects. The thread is contained in the thread pool or deleted depends on the server implementation.
  ```
      
###### 3.The Web Server passes the request to the corresponding servlet.
###### 4.The Servlet processes the request and generates the response in the form of output.
###### 5.The Servlet sends the response back to the webserver.
###### 6.The Web Server sends the response back to the client and the client browser displays it on the screen.
- 
**code:**
  index.html
  ```
  <!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<form action="add" method="post">
		Enter first number: <input type="text" name="num1" /><br/>
		Enter second number: <input type="text" name="num2" /><br/>
		<input type="submit" />
		
		<!--after hitting submit button, my url changes to "http://localhost:8080/DemoApp/add?num1=1&num2=1" -->
		<!-- by default it uses get method -->
		<!-- if i don't want to send this query params(?num1=1&num2=1), we can use post method. and rest all same, then this query params will not show-->
		
	</form>
</body>
</html> 
```

**AddServerlet.java**

```
package com.aqib;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class AddServerlet extends HttpServlet {
	//we are creating a servelet. and a serve let is a component of server
	//server provide services.
	// that's why we need to call a method service
//	public void service(HttpServletRequest req, HttpServletResponse res) throws IOException {
//		String num1 = req.getParameter("num1");
//        String num2 = req.getParameter("num2");
//		
//		int ans = Integer.parseInt(num1)+Integer.parseInt(num2);
//		
//		PrintWriter out = res.getWriter(); //PrintWriter class is used to send character text to the client (typically a web browser).
//		//res.getWriter(); provides an output stream
//		out.println("Sum is: "+ans);
//	}
	// both get and post request can access this service method.
	// if we want to control this, we can control. means if we want this method should call only for POST request then we can use "doPost" method
	// if we want should call only for GET request then we can use "doGet" method
	// This doGet and doPost methods are part of service method. so our request passed through service method and then we check internally. you can check service method of class HttpServlet.
	
	//eg.
	// woth only  for post method
	public void doPost(HttpServletRequest req, HttpServletResponse res) throws IOException {
		String num1 = req.getParameter("num1");
        String num2 = req.getParameter("num2");
		
		int ans = Integer.parseInt(num1)+Integer.parseInt(num2);
		
		PrintWriter out = res.getWriter(); //PrintWriter class is used to send character text to the client (typically a web browser).
		//res.getWriter(); provides an output stream
		out.println("Sum is: "+ans);
	}
	
}
```
**web.xml**
```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd" id="WebApp_ID" version="4.0">
  <display-name>DemoApp</display-name>
  
 
 	<servlet>
 		<servlet-name>abc</servlet-name>
 		<servlet-class>com.aqib.AddServerlet</servlet-class>
 	</servlet>
 	<servlet-mapping>
 		<servlet-name>abc</servlet-name>
 		<url-pattern>/add</url-pattern>
 	</servlet-mapping>
 	
</web-app>
```
# RequestDispatcher | Calling a Servlet from another Servlet 
The RequestDispatcher interface provides the facility of dispatching the request to another resource it may be html, servlet or jsp. This interface can also be used to include the content of another resource also. It is one of the way of servlet collaboration.

RequestDispatcher(it contains two methods (i) forward (ii) include)

**forward:** Forwards a request from a servlet to another resource (servlet, JSP file, or HTML file) on the server.
![forward](https://github.com/user-attachments/assets/1208ae64-c514-475f-8442-d038fb483662)

As you see in the above figure, response of second servlet is sent to the client. Response of the first servlet is not displayed to the user.

**include:** Includes the content of a resource (servlet, JSP page, or HTML file) in the response.

As you can see in the above figure, response of second servlet is included in the response of the first servlet that is being sent to the client.

index.html
```
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<form action="add" method="get">
		Enter first number: <input type="text" name="num1" /><br/>
		Enter second number: <input type="text" name="num2" /><br/>
		<input type="submit" />
		
		<!--after hitting submit button, my url changes to "http://localhost:8080/DemoApp/add?num1=1&num2=1" -->
		<!-- by default it uses get method -->
		<!-- if i don't want to send this query params(?num1=1&num2=1), we can use post method. and rest all same, then this query params will not show-->
		
	</form>
</body>
</html>
```

AddServlet.java
```
package com.aqib;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class AddServerlet extends HttpServlet {
	
	public void doGet(HttpServletRequest req, HttpServletResponse res) throws IOException, ServletException {
		String num1 = req.getParameter("num1");
        String num2 = req.getParameter("num2");
		
		int ans = Integer.parseInt(num1)+Integer.parseInt(num2);
		
		
		//for dispatching request from one servlet to another we have 2 methods
		//1. RequestDispatcher(it contains two methods (i) forward (ii) include
		
		//also we can pass data in the req obj
		req.setAttribute("ans", ans);
		RequestDispatcher rd = req.getRequestDispatcher("sq"); // we have give path "Sq"
		rd.forward(req, res);
		
		PrintWriter out = res.getWriter(); //PrintWriter class is used to send character text to the client (typically a web browser).
		//res.getWriter(); provides an output stream
		out.println("Sum is: "+ans);
	}
	
}
```

SqServlet.java
```
package com.aqib;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;



public class SqServlet extends HttpServlet {
	public void doGet(HttpServletRequest req, HttpServletResponse res) throws IOException {
		
		int ans = (int) req.getAttribute("ans"); // convert into int bcz return an object
		PrintWriter out = res.getWriter();
		out.println("Result is: "+ans*ans);
	}
}
```

web.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd" id="WebApp_ID" version="4.0">
  <display-name>DemoApp</display-name>
  
 
 	<servlet>
 		<servlet-name>abc</servlet-name>
 		<servlet-class>com.aqib.AddServerlet</servlet-class>
 	</servlet>
 	<servlet-mapping>
 		<servlet-name>abc</servlet-name>
 		<url-pattern>/add</url-pattern>
 	</servlet-mapping>
 	
 	 	<servlet>
 		<servlet-name>pqr</servlet-name>
 		<servlet-class>com.aqib.SqServlet</servlet-class>
 	</servlet>
 	<servlet-mapping>
 		<servlet-name>pqr</servlet-name>
 		<url-pattern>/sq</url-pattern>
 	</servlet-mapping>
</web-app>
```

**Note:** prefer below link for better understanding also it include include and forward method combination.

Link: https://www.javatpoint.com/requestdispatcher-in-servlet

# SendRedirect in servlet
The sendRedirect() method of HttpServletResponse interface can be used to redirect response to another resource, it may be servlet, jsp or html file.

It accepts relative as well as absolute URL.

It works at client side because it uses the url bar of the browser to make another request. So, it can work inside and outside the server.

Note: Basically in RequestDispatcher() we have dispatch our data from one servlet to another. it is happens only server side of same code base. 
- eg: client-->req1-->servlet1(xyz.java)--->req1--->servlet2(abc.java)--->client (servlet1 and servlet 2 will be in samecodebase and send same req,res object)
- here url is not changed, also my request url will always show for servlet1

But in SendRedirect() method our request goes from goes from one resource(codebase) to another resouce. eg: E-comm website to paypal payment gateway.
- Main point here is that here url would be change.
- client-->req1-->servlet1(xyz.java) of resource 1---->client--->req2--->servlet2(mno.java) of resource 2--->client

  ![Screenshot 2024-10-26 142051](https://github.com/user-attachments/assets/c754e361-4d15-49d2-964f-11d440713359)

  - in above image you can see that flow, and if we want to send data from one servlet to another using RequestDispatcher() we can use (req,res) objects. and here url will always show of S1.
  - but if we want to send data from one resource to another then we need to use **session managemnet**. here url will be change for resources.

### Difference between `forward()` and `sendRedirect()` Methods in Servlets

| Feature                        | `forward()` Method                                                                                   | `sendRedirect()` Method                                                                 |
|--------------------------------|------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
| Execution Side                 | Works at the **server side**                                                                         | Works at the **client side**                                                             |
| Request and Response Objects   | Forwards the **same request and response objects** to another servlet                                | **Sends a new request** each time                                                       |
| Scope                          | Works **within the server** only                                                                    | Can be used both **within and outside the server**                                       |
| Usage Example                  | `request.getRequestDispatcher("servlet2").forward(request, response);`                               | `response.sendRedirect("servlet2");`                                                     |

**Session Tracking:** Session Tracking is a way to maintain state (data) of an user. It is also known as session management in servlet.
- we have use session tracking with sendRedirect(), when we need to pass data from one servlet to another
- There are 3 methods of session management
- 1. URL rewriting
  2. session
  3. cookie

**Example 1: sendRedirect with URL rewriting**

AddServerlet.java

```
package com.aqib;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class AddServerlet extends HttpServlet {
	
	public void doGet(HttpServletRequest req, HttpServletResponse res) throws IOException, ServletException {
		String num1 = req.getParameter("num1");
        	String num2 = req.getParameter("num2");
		
		int ans = Integer.parseInt(num1)+Integer.parseInt(num2);

		res.sendRedirect("sq?ans="+ans);  // it will change our url
		// in this example we have use redirection with url rewriting 
	}
	
}

```
SqServlet.java

```
package com.aqib;
import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class SqServlet extends HttpServlet {
	public void doGet(HttpServletRequest req, HttpServletResponse res) throws IOException {
				
		int ans = Integer.parseInt(req.getParameter("ans"));
		PrintWriter out = res.getWriter(); 
		out.println("Result is: "+ans*ans);
	}
}

```

**web.xml and index.html file same as above RequesDispatcher() example.**

**2. session** 

2 Http session

- if we want to send this data into multiple servlets, so instead of sending this data via url, use a session
- container creates a session id for each user.The container uses this id to identify the particular user.
- HttpSession is an interface and its implementation provides TomCat
- genrally it is use for login & signup. session will be persist for multiple servlets or url(bcz use SendRedirect())

  AddServlet.java
  
```
package com.aqib;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

public class AddServerlet extends HttpServlet {
	
	public void doGet(HttpServletRequest req, HttpServletResponse res) throws IOException, ServletException {
		String num1 = req.getParameter("num1");
    	String num2 = req.getParameter("num2");
	
	int ans = Integer.parseInt(num1)+Integer.parseInt(num2);
	
	HttpSession session = req.getSession(); //req.getSession() Returns the current session associated with this request,or if the request does not have a session, creates one.
	
	session.setAttribute("ans", ans);
	res.sendRedirect("sq");
	}
}

```

SqServlet.java

```
public class SqServlet extends HttpServlet {
	public void doGet(HttpServletRequest req, HttpServletResponse res) throws IOException {
				
		HttpSession session = req.getSession();
		int ans = (int) session.getAttribute("ans");

		//session.removeAttribute("ans"); //to remove session for "ans"
		PrintWriter out = res.getWriter();
		out.println("Result is:"+ ans*ans);
		
	}
}
```

**web.xml and index.html file same as above RequesDispatcher() example.**

**3 Cookies:** A cookie is a small piece of information that is persisted between the multiple client requests.

**How Cookie works**

By default, each request is considered as a new request. In cookies technique, we add cookie with response from the servlet. So cookie is stored in the cache of the browser. After that if request is sent by the user, cookie is added with request by default. Thus, we recognize the user as the old user.
![cookie](https://github.com/user-attachments/assets/2000ba8a-54f0-4f02-9ead-e3c03b326085)
**AddServlet.java**
```
public class AddServerlet extends HttpServlet {
	
	public void doGet(HttpServletRequest req, HttpServletResponse res) throws IOException, ServletException {
		String num1 = req.getParameter("num1");
    	String num2 = req.getParameter("num2");
	
	int ans = Integer.parseInt(num1)+Integer.parseInt(num2);
	
	Cookie cookie = new Cookie("ans",ans+""); //cookie is a class of Servlet
	//to convert into string, because this constructor accept both parameters as string
	res.addCookie(cookie);//now we have send this cookie to client in response object
	
	// and when user request then we have use this cookie in SqServlet 
	
	res.sendRedirect("sq");
	}
	
}
```
**SqServlet.java**
```
public class SqServlet extends HttpServlet {
	public void doGet(HttpServletRequest req, HttpServletResponse res) throws IOException {
				
		int ans=0;
		Cookie cookies[] = req.getCookies(); // now we will get cookies in request object and it's give all cookies in form of array
		for(Cookie c:cookies) {
			if(c.getName().equals("ans")) {
				ans=Integer.parseInt(c.getValue());
			}
		}
		
		PrintWriter out = res.getWriter();
		out.println("Result is:"+ ans*ans);
		
	}
}
```
**web.xml and index.html file same as above RequesDispatcher() example.**

# ServletConfig and ServletContext
ServletConfig and ServletContext, both are objects created at the time of servlet initialization and used to provide some initial parameters(eg: DB url, special keys) or configuration information to the servlet. But, the difference in that information shared by ServletConfig is for a specific servlet, while information shared by ServletContext is available for all servlets in the web application.

ServletConfig and ServletContext both are interfaces

**ServletConfig**: ServletConfig is for a particular servlet, that means one should store servlet specific information in web.xml and retrieve them using this object.
- Parameters of servletConfig are present as name-value pair in <init-param> inside <servlet>.

eg: web.xml
```
	<servlet>
 		<servlet-name>abc</servlet-name>
 		<servlet-class>com.aqib.AddServerlet</servlet-class>
 		// this is only for AddServerlet.java
 		<init-param>  
			<param-name>name</param-name>  
			<param-value>AddServlet</param-value>  
		</init-param> 
 	</servlet>
 	<servlet-mapping>
 		<servlet-name>abc</servlet-name>
 		<url-pattern>/add</url-pattern>
 	</servlet-mapping>
```
AddServlet.java
```
public class AddServerlet extends HttpServlet {
	
	public void doGet(HttpServletRequest req, HttpServletResponse res) throws IOException, ServletException {
		
		PrintWriter out = res.getWriter();
		ServletConfig cg = getServletConfig(); // this is acceptable only for AddServlet.java as we mention in web.xml
		out.println("Hii "+cg.getInitParameter("name"));
	}
	
}
```

**ServletContext:** ServletContext is the object created by Servlet Container to share initial parameters or configuration information to the whole application.
- Parameters of servletContext are present as name-value pair in <context-param> which is outside of <servlet> and inside <web-app>

web.xml
```
	//available for complete app
	<web-app>
	 	<context-param>
	 		<param-name>name</param-name>
	 		<param-value>Aqib</param-value>
	 	</context-param>
	</web-app>
```
**AddServlet.java or koi b java file me use kr skte hai**
```
public class AddServerlet extends HttpServlet {
	
	public void doGet(HttpServletRequest req, HttpServletResponse res) throws IOException, ServletException {
		
		PrintWriter out = res.getWriter();

		ServletContext ctx = getServletContext(); // this is acceptable by complete application
		out.println("Hii "+ctx.getInitParameter("name"));
	}
	
}
```
# Servlet Annotation Configuration 

- using servlet annotation, we don't need to maintain web.xml
```
@WebServlet("/add"); // using this we don't need routing in web.xml here this class will run on "/add"
public class AddServerlet extends HttpServlet {
	
	public void doGet(HttpServletRequest req, HttpServletResponse res) throws IOException, ServletException {
		
		PrintWriter out = res.getWriter();
		ServletConfig cg = getServletConfig(); // this is acceptable only for AddServlet.java as we mention in web.xml
		out.println("Hii "+cg.getInitParameter("name"));
		
		
		ServletContext ctx = getServletContext(); // this is acceptable by complete application
		out.println("Hii "+ctx.getInitParameter("name"));
	}
	
}
```

# JSP

## What and Why?
In Java, JSP stands for Jakarta Server Pages( (JSP; formerly JavaServer Pages)). It is a server-side technology that is used for creating web applications. It is used to create dynamic web content. JSP consists of both HTML tags and JSP tags. **In this, JSP tags are used to insert JAVA code into HTML pages.** It is an advanced version of Servlet Technology i.e. a web-based technology that helps us to create dynamic and platform-independent web pages. In this, Java code can be inserted in HTML/ XML pages or both. **JSP is first converted into a servlet by the JSP container before processing the client’s request.** JSP has various features like JSP Expressions, JSP tags, JSP Expression Language, etc.

JSP allows embedding Java code in HTML pages, making it easier to build interactive web applications.

## How JSP Translated into Servlets ?
![Screenshot 2024-10-29 015511](https://github.com/user-attachments/assets/f787cb0f-67d4-456c-9eb4-2ab0779b0267)

- **scriptlet tag (<% service method inside this, and it's provide by default all objects like HttpServletRequest in form of request %>)**
- **Declaration tag (<%! all declared variables etc, that are present on outside service method and used inside servide method %>)** like declre variable whose are not in service
- **Expression tag(<%= expression %>)**  an expression tag is used to evaluate a Java expression and convert the result into a string, which is then inserted into the output of the JSP page. This is a convenient way to embed dynamic content directly within the HTML structure of a JSP file.
- **Declarative tag(<%page import="java.util.*","java.util.Date" %>)** used for import libraries and other stuff;

**Example:**
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>

	<%@page import="java.util.*"%> <!-- This is an Directive tag, used to import etc -->
	<h1 style="font-size: 3rem; background-color: red;">hello world</h1>

	<%! int global = 5; // this is a Declaration tag (outside and above service method) %>

	<%
		int i=3;
		out.println(i+4); //this is a scriptlet tag, (or it is service method)
		out.println(global+i);
		
	%>
	<br /> The value of Global variable is:<%= global %> and this is an example of <b>Expression tag</b>
</body>
</html>
```

### JSP Directive Tag:
There are three types of directives
- page directive
- include directive
- taglib directive

**Syntax of JSP Directive**
```
<%@ directive attribute="value" %>
```
#### **1. JSP page directive**

The page directive defines attributes that apply to an entire JSP page.

**Syntax of JSP page directive**
```
<%@ page attribute="value" %>
```
**Attributes of JSP page directive**

**import** - The import attribute is used to import class,interface or all the members of a package.It is similar to import keyword in java class or interface.

**contentType** - The contentType attribute defines the MIME(Multipurpose Internet Mail Extension) type of the HTTP response.The default value is "text/html;charset=ISO-8859-1".

**extends** - The extends attribute defines the parent class that will be inherited by the generated servlet.It is rarely used.

**info** - This attribute simply sets the information of the JSP page which is retrieved later by using getServletInfo() method of Servlet interface.

Example of info attribute
```
<html>  
<body>  
  
<%@ page info="composed by Sonoo Jaiswal" %>  
Today is: <%= new java.util.Date() %>  
  
</body>  
</html>
```
The web container will create a method getServletInfo() in the resulting servlet.For example:
```
public String getServletInfo() {  
  return "composed by Sonoo Jaiswal";   
}
```
**buffer** - The buffer attribute sets the buffer size in kilobytes to handle output generated by the JSP page.The default size of the buffer is 8Kb.

if you dont want to send data to client untill buffer is full, you can use buffer. it's allows more content to be written before anything is actually sent back to the client.
```
<%@ page buffer="16kb" %>
```
**language** - The language attribute specifies the scripting language used in the JSP page. The default value is "java".

**isELIgnored(ture|false):**

**isThreadSafe** - Servlet and JSP both are multithreaded.If you want to control this behaviour of JSP page, you can use isThreadSafe attribute of page directive.The value of isThreadSafe value is true.If you make it false, the web container will serialize the multiple requests, i.e. it will wait until the JSP finishes responding to a request before passing another request to it.If you make the value of isThreadSafe attribute like:
```
<%@ page isThreadSafe="false" %>
```
**autoFlush(true|false)** - to flush buffer.

**session(true|false)**

**pageEncoding**

**errorPage**- The errorPage attribute is used to define the error page, if exception occurs in the current page, it will be redirected to the error page.
```
<%@ page errorPage="myerrorpage.jsp" %>
```
**isErrorPage**-The isErrorPage attribute is used to declare that the current page is the error page.
Note: The exception object can only be used in the error page.
```
//myerrorpage.jsp  
<html>  
<body>  
  
<%@ page isErrorPage="true" %>  
  
 Sorry an exception occured!<br/>  
The exception is: <%= exception %>  
  
</body>  
</html>
```
### Jsp Include Directive
Basically it is used to include another pages or resources in current page.
```
<%@ include file file="header.html" %>
```
### JSP Taglib directive
if you want to use external tag library.
**Syntax JSP Taglib directive**
```
<%@ taglib uri="uriofthetaglibrary" prefix="prefixoftaglibrary" %>
```
```
<html>  
<body>  
  
<%@ taglib uri="http://www.javatpoint.com/tags" prefix="mytag" %>  
  
<mytag:currentDate/>   //means this currentDate tag belongs to mytag, which url is given
  
</body>  
</html>
```

### . Implicit Object in JSP or (built-in objects gives JSP) and can be used in Scriptlet and Expression tag

- **request** (HttpServletRequest)
- **response** (HttpServletResponse)
- **pageContext** (PageContext) : basically here we can set context for page
```
pageContext.setAttribute("name", "aqib",pageContext.SESSION_SCOPE); // by default it scopes in same page and we can get using getAttribute
		//but we can change the scope of its using third parameter, like here we hane change scope to session scope
```
now getting in another page by session
```
 <%  String name = (String) pageContext.getAttribute("User", PageContext.SESSION_SCOPE); out.print("Hey!!!" + name); %>
```
- **session** (HttpSession)
- **out** (JspWriter) ~ PrintWriter object
- **application** (ServletContext ) : provide context for complete app
- **config** (ServletConfig) : Provide context only specified page like we have seen in web.xml

### Exception Handling in JSP:
In java, we can handle exception by using try-catch. here also we can use it. but the best thing is we have used given Conventions. follow below examples
> we have created an error.jsp file and when any exception is error then error.jsp page will handle it

index.jsp
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8" errorPage="error.jsp"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>

	<%
	 int k=9/0; // it will give unchecked exception and it call to error.jsp page to handle this exception
	%>
</body>
</html>
```
error.jsp
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8" isErrorPage="true" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body bgcolor="red">

<h1>Error:</h1><%= exception %>

</body>
</html>
```
## Connection of JSP with JDBC
**Important:** here connection jar file should be copy into **webapp>WEB-INF>lib**.(lib me jana chahye)
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
	<%@ page import="java.sql.*" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body> 

	<%
	 	String url = "jdbc:mysql://localhost:3306/trainTicketBooking";
		String user = "root";
		String password="Aqib8267@";
		
		String query = "select * from user where id=1";
		Class.forName("com.mysql.jdbc.Driver");
		Connection conn = DriverManager.getConnection(url,user,password);
		Statement st = conn.createStatement();
		ResultSet rs = st.executeQuery(query);
		rs.next();
	%>
	Name:<%= rs.getString("name") %> <br/>
	Email:<%= rs.getString("email") %>
	<%
		rs.close();  
		st.close();
		
	%>
</body>
</html>
```
## MVC Architecture:
![Screenshot 2024-10-30 013226](https://github.com/user-attachments/assets/28e5cbae-55b3-4457-899c-8310437869cf)

![Screenshot 2024-10-30 013146](https://github.com/user-attachments/assets/29ff0544-6ff9-495e-9543-907f7f5fb029)


# JSTL (Java serverpage tag library)

**Core tag library**

> AddServlet.java
```
package com.aqib;

import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/add")

public class AddServlet extends HttpServlet{
	public void service(HttpServletRequest req,HttpServletResponse res) throws ServletException, IOException {
		
		String name = "Aqib";
		
		List<Student>st = new ArrayList<>();
		st.add(new Student(1,"Aqib")); st.add(new Student(2,"Kashif"));
		
		
		req.setAttribute("list", st);
		RequestDispatcher rd = req.getRequestDispatcher("NewFile.jsp");
		rd.forward(req, res);
	}
}
```
> NewFile.jsp
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
	
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
	
<!DOCTYPE html>  
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body> 
	Hello
	<%
		// 1st method to get servlet data
		//String name = request.getAttribute("label").toString(); 
		//out.println(name);
	%> 
	<br>
	<!-- 2nd method -->
	${list}
	 
	<br/>   
	<!-- using JSTL core library -->
	<c:forEach items="${list}" var="s">
		${s.getName()} <br/>
	</c:forEach>
</body>
</html>
```
```
> Student.java
```
package com.aqib;

public class Student {
	int rollno;
	String name;
	public Student(int rollno, String name) {
		super();
		this.rollno = rollno;
		this.name = name;
	}
	public int getRollno() {
		return rollno;
	}
	public void setRollno(int rollno) {
		this.rollno = rollno;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	@Override
	public String toString() { 
		return "Student [rollno=" + rollno + ", name=" + name + "]";
	}
	
}
```
