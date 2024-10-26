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
