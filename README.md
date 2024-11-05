# Struds1.3Demo" 
youtube video: 
https://www.youtube.com/watch?v=YcCsJtqI72A

# Steps: 
1. Open STS -> Create new Dynamic web project -> Enter project name(Struds1.3Demo) -> Next -> tick on Generate the web.xml deployment descriptor

2. Right click project -> configure -> convert to maven project -> click finish

3. open pom.xml to edit the file -> remove the entire "build" tag content. add dependencies in it

4. Add the following dependencies in it: 
    * servlet 3.0 (3.1.0)
    * struds core (1.3.10)
    * struds taglib (1.3.10)

5. Hence the pom.xml file would look like: 
    ```
    <dependencies>
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>javax.servlet-api</artifactId>
			<version>3.1.0</version>
			<scope>provided</scope>
		</dependency>
		<dependency>
			<groupId>org.apache.struts</groupId>
			<artifactId>struts-core</artifactId>
			<version>1.3.10</version>
		</dependency>
		<dependency>
			<groupId>org.apache.struts</groupId>
			<artifactId>struts-taglib</artifactId>
			<version>1.3.10</version>
		</dependency>
	</dependencies>
    ``` 
    Note: To format the file: click (ctrl + shift + F)

6. clean and build the project to add the maven dependencies to the path
    click on project -> clean -> (select the particular project) -> (select the radio) Build the entire workspace -> click Ok

## web deployment descriptor(web.xml)
1. inside src -> main -> webapp -> WEB-INF -> web.xml
2. remove the content of (web-file-list) tag
3. Register Struts action servlet
    * in struts 1.3, action servlet represent the controller in MVC model
```
<servlet>
    <servlet-name>action</servlet-name>
    <servlet-mapping>org.apache.struts.action.AcrionServlet</servlet-mapping>
</servlet>
```
4. struds config.xml is declared in initial parameters
```
<servlet>
    <init-param>
        <param-name>param</param-name>
        <param-value>/WEB-INF/struct-config.xml</param-value>
    </init-param>
</servlet>
```
5. we use (load-on-startup) tag to force the container to load and initialize the servlet startup and specify the servlet mapping for the action servlet
``` 
<load-on-startup>1</load-on-servlet>
```
6. 
```
<servlet-mapping>
        <servlet-name>action</servlet-name>
        <url-pattern>*.do</url-pattern>
    </servlet-mapping>
```
7. Hence the ultimate file would look like: 
``` 
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd" id="WebApp_ID" version="4.0">
  <display-name>Struts1.3Demo</display-name>
   <servlet>
        <servlet-name>action</servlet-name>
        <servlet-class>org.apache.struts.action.ActionServlet</servlet-class>
        <init-param>
            <param-name>param</param-name> 
            <param-value>/WEB-INF/struts-config.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>action</servlet-name>
        <url-pattern>*.do</url-pattern>
    </servlet-mapping>
  
</web-app>
```

## Struts configuration file(struts-config.xml)
1. Right click WEB-INF -> new -> xml file -> struts-config.xml
2. search "struts-config 1.3 dtd" and paste the dtd in this file
```
 <!DOCTYPE struts-config PUBLIC
       "-//Apache Software Foundation//DTD Struts Configuration 1.3//EN"
       "http://struts.apache.org/dtds/struts-config_1_3.dtd">
```
3. create the root element of file: (struts-config) tag
4. create the form-bean tag: (form-beans) tag
```
<form-beans>
    <form-bean name="helloForm" type="com.yash.form.HelloForm">
</form-beans>
```
5. define action mapping: action mapping contain action definitions
```
    <action-mappings>
		<action path="/hello" name="helloForm"
			type="info.java.tips.action.HelloAction" input="hello.jsp">
			<forward name="succsess" path="/welcome.jsp" />
		</action>
	</action-mappings>
```
6. configure the message bundling file: 
```
<message-resources
		parameter="info.java.tips.i18n.MessageBundle" />
```

7. Hence the entire file would look like this: 
``` 
<?xml version="1.0" encoding="UTF-8"?>

 <!DOCTYPE struts-config PUBLIC
   "-//Apache Software Foundation//DTD Struts Configuration 1.3//EN"
   "http://struts.apache.org/dtds/struts-config_1_3.dtd">

<struts-config>
	<form-beans>
		<form-bean name="helloForm"
			type="info.java.tips.form.HelloForm" />
	</form-beans>

	<action-mappings>

		<action path="/hello" name="helloForm"
			type="info.java.tips.action.HelloAction" input="hello.jsp">
			 
			<forward name="succsess" path="/welcome.jsp" />
		</action>

	</action-mappings>

	<message-resources
		parameter="info.java.tips.i18n.MessageBundle" />

</struts-config>
```

## MessageBundle.properties
1. right click on project -> new -> create package (info.java.tips.i18n)
2. create a new MessageBundle.properties file
3. write the messages here: 
```
hello.msg.err = Sorry, you are not Krishna...
info.java.tips.err =  Sorry, you are not Krishna...
```

## Struts form bean class
1. right click the project and inside the package named (info.java.tips.form) create the file HelloForm.java
2. create HelloForm class and extend ActionForm in it
3. create the hello pojo in this
```
package info.java.tips.form;

import org.apache.struts.action.ActionForm;

public class HelloForm extends ActionForm{
	private String name; 
	
	public String getName() {
		return name; 
	}
	
	public void setName(String name) {
		this.name = name; 
	}
}
```

## Struts Action class
1. Right click the project and inside the package named (info.java.tips.action) create the file HelloAction.java
    * Note: action represent the business logic
2. define the HelloAction class in it and extend Action class
3. in it write the execute function(you can also auto generate the code)
```
package info.java.tips.action;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.apache.struts.action.Action;
import org.apache.struts.action.ActionErrors;
import org.apache.struts.action.ActionForm;
import org.apache.struts.action.ActionForward;
import org.apache.struts.action.ActionMapping;
import org.apache.struts.action.ActionMessage;

import info.java.tips.form.HelloForm;

public class HelloAction extends Action {
	@Override
	public ActionForward execute(ActionMapping mapping, ActionForm form, HttpServletRequest request,
			HttpServletResponse response) throws Exception {
		// TODO Auto-generated method stub
		
		HelloForm helloForm = (HelloForm) form;
		ActionForward fw = mapping.getInputForward();
		
		if(helloForm != null && helloForm.getName().equalsIgnoreCase("Krishna")) {
			fw = mapping.findForward("success");
		} else {
			ActionErrors errs = new ActionErrors();
			errs.add("", new ActionMessage("hello.msg.err"));
			saveErrors(request, errs);
		}
		
		return super.execute(mapping, form, request, response);
	}
}
```

## jsp pagess
### hello.jsp
1. right click web content and create a new jsp file named hello.jsp file
2. import the taglib in your jsp page
```
<%@ taglib uri="http://struts.apache.org/tags-html" prefix="h" %>
```
3. add form
```
<h:form action="/hello">
    <h:text property="name" />
    <h:submit />
    <hr />
    <h:errors />
</h:form>   
```

### welcome.jsp
```
<%@ taglib uri="http://struts.apache.org/tags-bean" prefix="b" %>

Welcome '<b:write name="helloForm" property="name"/>' to javatips!
```

## run the application
1. Open the server(window -> show view -> other -> servers) click ok
2. Add tomcat v7.0 server in the servers terminal's popup
3. Click install JREs
4. select the installed jres
5. right click the server -> select add -> click on server and bring it to right