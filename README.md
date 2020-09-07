# ServletContext-vs-DispatcherServlet-vs-ContextLoadListener

#ServletConfig vs ServletContext : 

ServletConfig and ServletContext, both are objects created at the time of servlet initialization and used to provide some 
initial parameters or configuration information to the servlet. But, the difference lies in the fact that information shared by 
ServletConfig is for a specific servlet, while information shared by ServletContext is available for all servlets in the web application.

ServletConfig / ServletContext : 

1) ServletConfig is servlet specific / ServletContext is for whole application

2) Parameters of servletConfig are present as name-value pair in <init-param> inside <servlet> in web.xml. 
/ Parameters of servletContext are present as name-value pair in <context-param> which is outside of <servlet> and inside <web-app> in web.xml

3) ServletConfig object is obtained by getServletConfig() method. / ServletContext object is obtained by getServletContext() method.

4) Each servlet has got its own ServletConfig object. / ServletContext object is only one and used by different servlets of the application.

5) Use ServletConfig when only one servlet needs information shared by it. / Use ServletContext when whole application needs information shared by it

+ web.xml is a file that should reside in all J2EE web application. Its specification is defined by J2EE spec. Here you configure a general behaviour of your app. 
For example servlets, filters, security policy etc.

************************************************************

#ContextLoaderListener vs DispatcherServlet : 

In XML based Spring MVC configuration, you must have seen two declarations in web.xml file i.e. ContextLoaderListener and DispatcherServlet.

+ Root and child contexts : 

  Spring can have multiple contexts at a time. One of them will be root context, and all other contexts will be child contexts.

  All child contexts can access the beans defined in root context; but opposite is not true. Root context cannot access child contexts beans.

+ DispatcherServlet – Child application contexts : 

  DispatcherServlet is essentially a Servlet (it extends HttpServlet) whose primary purpose is to handle incoming web requests matching the configured URL pattern. 
  It take an incoming URI and find the right combination of controller and view. So it is the front controller.
  
  When you define a DispatcherServlet in spring configuration, you provide an XML file with entries of controller classes, views mappings etc. 
  using contextConfigLocation attribute.
  
  Web applications can define any number of DispatcherServlet entries. Each servlet will operate in its own namespace, loading its own application 
  context with mappings, handlers, etc.
  
  It means that each DispatcherServlet has access to web application context(root). Until specified, each DispatcherServlet creates own internal web application context.
  
+ ContextLoaderListener – Root application context : 
  
  ContextLoaderListener creates the root application context and will be shared with child contexts created by all DispatcherServlet contexts. 
  You can have only one entry of this in web.xml.
  
  The context of ContextLoaderListener contains beans that globally visible, like services, repositories, infrastructure beans, etc. After 
  the root application context is created, it’s stored in ServletContext as an attribute(All contexts are added to ServletContext).
  
+ summary : 

  Generally, you will define all MVC related beans (controller and views etc) in DispatcherServlet context, and all cross-cutting beans such as 
  security, transaction, services etc. at root context by ContextLoaderListener.

  Generally, this setup works fine because rarely you will need to access any MVC bean (from child context) into security related class (from root context). Mostly we use         security beans on MVC classes, and they can access it with above setup.
    
