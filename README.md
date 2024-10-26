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
