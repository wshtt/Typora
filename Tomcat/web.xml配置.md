[toc]



用于配置部署的 web 应用的信息

## 一、`conf/web.xml`

配置 web 应用的默认信息

#### servlet

默认使用的 servlet

```xml
// 默认
<servlet>
    <servlet-name>default</servlet-name>
    <servlet-class>org.apache.catalina.servlets.DefaultServlet</servlet-class>
    <init-param>
        <param-name>debug</param-name>
        <param-value>0</param-value>
    </init-param>
    <init-param>
        <param-name>listings</param-name>
        <param-value>false</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
</servlet>


// jsp 解析
<servlet>
    <servlet-name>jsp</servlet-name>
    <servlet-class>org.apache.jasper.servlet.JspServlet</servlet-class>
    <init-param>
        <param-name>fork</param-name>
        <param-value>false</param-value>
    </init-param>
    <init-param>
        <param-name>xpoweredBy</param-name>
        <param-value>false</param-value>
    </init-param>
    <load-on-startup>3</load-on-startup>
</servlet>
```

#### servlet-mapping 

拦截路径配置

```xml

// 默认  /
<servlet-mapping>
    <servlet-name>default</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>


// jsp  *.jsp    *.jspx
<!-- The mappings for the JSP servlet -->
<servlet-mapping>
    <servlet-name>jsp</servlet-name>
    <url-pattern>*.jsp</url-pattern>
    <url-pattern>*.jspx</url-pattern>
</servlet-mapping>
```

#### session-config

session 配置

```xml
// 默认 session 超时时间
<session-config>
    <session-timeout>30</session-timeout>
</session-config>
```

#### mime-mapping

mime-type 类型配置

```xml
<mime-mapping>
    <extension>123</extension>
    <mime-type>application/vnd.lotus-1-2-3</mime-type>
</mime-mapping>
```

#### welcome-file-list

欢迎页面

这些页面位于项目目录内

```xml
<welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
</welcome-file-list>
```



## 二、工程目录`WEB-INFO/web.xml`

#### context-param

ServletContext 初始化参数,可以在 servlet 使用

**ServletContext** ：web 容器启动的时候创建的对象，代表当前 web 应用

```xml
<context-param>
	<param-name>test_param</param-name>
    <param-value>test</param-value>
</context-param>
=======================================
在 servlet 中，直接使用HttpServletRequest 获取ServletContext 来获取初始化参数
=======================================
String value = req.get获取ServletContext().getInitParamete("test_param")

```

#### session-config

默认不需要配置

会话配置，cookie 与 session 相关，会覆盖 `conf/web.xml` 中的配置

```xml
<session-config>
    // session 超时时间 /分钟
    <session-timeout>30</session-timeout>
    
    // cookie 配置
    <cookie-config>
        // cookie 名称
    	<name>JESSIONID</name>
        // 域名
        <domain>localhost</domain>
        // 根路径
        <path>/</path>
        // 注释信息
        <comment>Session Cookie</comment>
        // cookie 只能通过 http 进行访问， 其他方式无效
        <http-only>true</http-only>
        // 是否只能接收 https
        <secure>false</secure>
        // cookie 有效期， /秒
        <max-age>3600</max-age>
    </cookie-config>
    
    // 跟踪 session 会话方式，以什么来表明这是一次会话。还有URL 后添加 jessionid=111的方式，SSL  会话标识
    <tracking-mode>COOKIE</tracking-mode>
</session-config>
```

#### servlet 与 servlet-mapping

servlet 与 servlet 映射配置

```xml
// servlet 
<servlet>
    // servlet 名称
    <servlet-name>myServlet</servlet-name>
    // servlet 实现类
    <servlet-class>com.qianji.web.MyServlet</servlet-class>
    
    // 初始化参数，只针对当前 Servlet 有效
    <init-param>
        <param-name>debug</param-name>
        <param-value>0</param-value>
    </init-param>
    <init-param>
        <param-name>listings</param-name>
        <param-value>false</param-value>
    </init-param>
    
    // Servlet 加载顺序，如果小于 0、不配置，则应用启动不会在启动时加载，在第一次被访问时加载。 如果大于等于 0，则会在应用启动时加载
    <load-on-startup>1</load-on-startup>
    
    // 当前 Servlet 是否可用，默认为 true。如果为 false，则该 Servlet 不会接受请求 404
    <enable>true</enable>
</servlet>


// servlet 映射
<servlet-mapping>
    // servlet 名字
    <servlet-name>default</servlet-name>
    // 请求路径 ,路径可以配置多个
    <url-pattern>/</url-pattern>
    <url-pattern>/add</url-pattern>
</servlet-mapping>
```

#### listener 监听器配置

listener 用于监听 servlet 中的事件，例如context、request、session 对象的创建，修改，删除，并触发响应事件。

listener 是观察者模式的具体实现，

```xml
<listener>
	<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
```

#### filter 过滤器

filter 用于配置 web 应用过滤器，用来过滤资源请求和相应。

filter 常用于 身份认证、日志、加密、数据转换等

```xml
<filter>
    // 名字
	<filter-name>myFilter</filter-name>
    // 类路径
    <filter-class>com.qianji.web.MyFilter</filter-class>
    // 是否支持异步
    <async-supported>true</async-supported>
    
    // 初始化参数
    <init-param>
        <param-name>language</param-name>
        <param-value>CN</param-value>
    </init-param>
</filter>

//  filter 路径映射
<filter-mapping>
    // Filter 名
	<filter-name>myFilter</filter-name>
    // 拦截路径
	<url-pattern>/*</url-pattern>
</filter-mapping>
```

#### welcome-file-list  首页

首页，欢迎页面

先找第一个，没有则找第二个，以此类推

```xml
<welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>login.jsp</welcome-file>
</welcome-file-list>
```

#### error-page  错误页面

发生异常自动跳转此页面 404 500 等

与首页放在同一路径，项目根路径

```xml

// 1. 根据错误码跳转
<error-page>
    // 错误码
    <error-code>404</error-code>
    // 错误码跳转的页面
	<location>/404.html</location>
</error-page>
    
<error-page>
    <error-code>500</error-code>
	<location>/500.html</location>
</error-page>

// 2. 根据异常类型跳转
<error-page>
    // 异常类型
	<exception-type>java.lang.Exception</exception-type>
    <location>/500.html</location>
</error-page>
```

