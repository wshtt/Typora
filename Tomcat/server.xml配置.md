[toc]



##### `conf/*` 中的配置文件



## server.xml

tomcat 服务器的核心配置文件，包含了 Tomcat 的 Servlet 容器（Catalina）的所有配置。

默认 server.xml 文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--
  Licensed to the Apache Software Foundation (ASF) under one or more
  contributor license agreements.  See the NOTICE file distributed with
  this work for additional information regarding copyright ownership.
  The ASF licenses this file to You under the Apache License, Version 2.0
  (the "License"); you may not use this file except in compliance with
  the License.  You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License.
-->
<!-- Note:  A "Server" is not itself a "Container", so you may not
     define subcomponents such as "Valves" at this level.
     Documentation at /docs/config/server.html
 -->
<Server port="8005" shutdown="SHUTDOWN">
  <Listener className="org.apache.catalina.startup.VersionLoggerListener" />
  <!-- Security listener. Documentation at /docs/config/listeners.html
  <Listener className="org.apache.catalina.security.SecurityListener" />
  -->
  <!--APR library loader. Documentation at /docs/apr.html -->
  <Listener className="org.apache.catalina.core.AprLifecycleListener" SSLEngine="on" />
  <!-- Prevent memory leaks due to use of particular java/javax APIs-->
  <Listener className="org.apache.catalina.core.JreMemoryLeakPreventionListener" />
  <Listener className="org.apache.catalina.mbeans.GlobalResourcesLifecycleListener" />
  <Listener className="org.apache.catalina.core.ThreadLocalLeakPreventionListener" />

  <!-- Global JNDI resources
       Documentation at /docs/jndi-resources-howto.html
  -->
  <GlobalNamingResources>
    <!-- Editable user database that can also be used by
         UserDatabaseRealm to authenticate users
    -->
    <Resource name="UserDatabase" auth="Container"
              type="org.apache.catalina.UserDatabase"
              description="User database that can be updated and saved"
              factory="org.apache.catalina.users.MemoryUserDatabaseFactory"
              pathname="conf/tomcat-users.xml" />
  </GlobalNamingResources>

  <!-- A "Service" is a collection of one or more "Connectors" that share
       a single "Container" Note:  A "Service" is not itself a "Container",
       so you may not define subcomponents such as "Valves" at this level.
       Documentation at /docs/config/service.html
   -->
  <Service name="Catalina">

    <!--The connectors can use a shared executor, you can define one or more named thread pools-->
    <!--
    <Executor name="tomcatThreadPool" namePrefix="catalina-exec-"
        maxThreads="150" minSpareThreads="4"/>
    -->


    <!-- A "Connector" represents an endpoint by which requests are received
         and responses are returned. Documentation at :
         Java HTTP Connector: /docs/config/http.html
         Java AJP  Connector: /docs/config/ajp.html
         APR (HTTP/AJP) Connector: /docs/apr.html
         Define a non-SSL/TLS HTTP/1.1 Connector on port 8080
    -->
    <Connector port="8081" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
    <!-- A "Connector" using the shared thread pool-->
    <!--
    <Connector executor="tomcatThreadPool"
               port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
    -->
    <!-- Define an SSL/TLS HTTP/1.1 Connector on port 8443
         This connector uses the NIO implementation. The default
         SSLImplementation will depend on the presence of the APR/native
         library and the useOpenSSL attribute of the
         AprLifecycleListener.
         Either JSSE or OpenSSL style configuration may be used regardless of
         the SSLImplementation selected. JSSE style configuration is used below.
    -->
    <!--
    <Connector port="8443" protocol="org.apache.coyote.http11.Http11NioProtocol"
               maxThreads="150" SSLEnabled="true">
        <SSLHostConfig>
            <Certificate certificateKeystoreFile="conf/localhost-rsa.jks"
                         type="RSA" />
        </SSLHostConfig>
    </Connector>
    -->
    <!-- Define an SSL/TLS HTTP/1.1 Connector on port 8443 with HTTP/2
         This connector uses the APR/native implementation which always uses
         OpenSSL for TLS.
         Either JSSE or OpenSSL style configuration may be used. OpenSSL style
         configuration is used below.
    -->
    <!--
    <Connector port="8443" protocol="org.apache.coyote.http11.Http11AprProtocol"
               maxThreads="150" SSLEnabled="true" >
        <UpgradeProtocol className="org.apache.coyote.http2.Http2Protocol" />
        <SSLHostConfig>
            <Certificate certificateKeyFile="conf/localhost-rsa-key.pem"
                         certificateFile="conf/localhost-rsa-cert.pem"
                         certificateChainFile="conf/localhost-rsa-chain.pem"
                         type="RSA" />
        </SSLHostConfig>
    </Connector>
    -->

    <!-- Define an AJP 1.3 Connector on port 8009 -->
    <!--
    <Connector protocol="AJP/1.3"
               address="::1"
               port="8009"
               redirectPort="8443" />
    -->

    <!-- An Engine represents the entry point (within Catalina) that processes
         every request.  The Engine implementation for Tomcat stand alone
         analyzes the HTTP headers included with the request, and passes them
         on to the appropriate Host (virtual host).
         Documentation at /docs/config/engine.html -->

    <!-- You should set jvmRoute to support load-balancing via AJP ie :
    <Engine name="Catalina" defaultHost="localhost" jvmRoute="jvm1">
    -->
    <Engine name="Catalina" defaultHost="localhost">

      <!--For clustering, please take a look at documentation at:
          /docs/cluster-howto.html  (simple how to)
          /docs/config/cluster.html (reference documentation) -->
      <!--
      <Cluster className="org.apache.catalina.ha.tcp.SimpleTcpCluster"/>
      -->

      <!-- Use the LockOutRealm to prevent attempts to guess user passwords
           via a brute-force attack -->
      <Realm className="org.apache.catalina.realm.LockOutRealm">
        <!-- This Realm uses the UserDatabase configured in the global JNDI
             resources under the key "UserDatabase".  Any edits
             that are performed against this UserDatabase are immediately
             available for use by the Realm.  -->
        <Realm className="org.apache.catalina.realm.UserDatabaseRealm"
               resourceName="UserDatabase"/>
      </Realm>

      <Host name="localhost"  appBase="webapps"
            unpackWARs="true" autoDeploy="true">

        <!-- SingleSignOn valve, share authentication between web applications
             Documentation at: /docs/config/valve.html -->
        <!--
        <Valve className="org.apache.catalina.authenticator.SingleSignOn" />
        -->

        <!-- Access log processes all example.
             Documentation at: /docs/config/valve.html
             Note: The pattern used is equivalent to using pattern="common" -->
        <Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
               prefix="localhost_access_log" suffix=".txt"
               pattern="%h %l %u %t &quot;%r&quot; %s %b" />

      </Host>
    </Engine>
  </Service>
</Server>

```



### Server标签

```xml
// 根标签，用于创建一个 server 实例，默认实现类是org.apache.catalina.core.StandardServer.
// port : 端口号
// shutdown : 关闭命令    意思是监听关闭 tomcat 命令的端口号
<Server port="8005" shutdown="SHUTDOWN">
    <Listener></Listener>
    <Listener></Listener>
    <Listener></Listener>
    <Listener></Listener>
    <Listener></Listener>
    <GlobalNamingResources></GlobalNamingResources>
    <Service></Service>
    <Service></Service>
</Server>
```

#### Listener 标签

Server 标签内有五个 Listener 标签，五个监听器

```xml
// 1. 用于以日志的形式输出的服务器、操作系统、jvm 版本的信息
<Listener className="org.apache.catalina.startup.VersionLoggerListener" />

  <!-- Security listener. Documentation at /docs/config/listeners.html
  <Listener className="org.apache.catalina.security.SecurityListener" />
  -->
  <!--APR library loader. Documentation at /docs/apr.html -->

// 2. 用于服务器启动加载、服务器停止销毁 Apr，如果没用安装 Apr 库，则会输出日志，不影响 tomcat 启动
<Listener className="org.apache.catalina.core.AprLifecycleListener" SSLEngine="on" />
  <!-- Prevent memory leaks due to use of particular java/javax APIs-->

// 3. 用于避免 jre 内存泄漏问题
<Listener className="org.apache.catalina.core.JreMemoryLeakPreventionListener" />

// 4. 用于服务器启动加载、服务器停止销毁 全局命名服务
<Listener className="org.apache.catalina.mbeans.GlobalResourcesLifecycleListener" />

// 5. 用于在 Context 停止时重建 Executor （线程池） 中的线程，以避免 ThreadLocal 相关的内存泄漏问题
  <Listener className="org.apache.catalina.core.ThreadLocalLeakPreventionListener" />
```

#### GlobalNamingResources 标签

全局命名服务

```xml
  <GlobalNamingResources>
    <!-- Editable user database that can also be used by
         UserDatabaseRealm to authenticate users
    -->
    <Resource name="UserDatabase" auth="Container"
              type="org.apache.catalina.UserDatabase"
              description="User database that can be updated and saved"
              factory="org.apache.catalina.users.MemoryUserDatabaseFactory"
              pathname="conf/tomcat-users.xml" />
  </GlobalNamingResources>
```

#### Service 标签

用于创建 Service 实例，默认实现类是org.apache.catalina.core.StandardServer

一个Server 服务器，可以包含多个 Service 服务，==请求根据端口找到指定的service==

```xml
// name : Service 的名词
<Service name="Catalina">
    <Executor name="tomcatThreadPool" namePrefix="catalina-exec-"
              maxThreads="150" minSpareThreads="4"/>

    <Connector port="8081" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
    <Connector />

    <Engine name="Catalina" defaultHost="localhost"></Engine>
</Service>
```

##### Executor 标签

线程池

默认形况下，service 并未添加==共享线程池==配置，

**共享线程池**：配置了共享线程池后，当前 service 下的多个  Connector 就可以共同使用该线程池，如果不配置共享线程池，那每个 Connector 都只在使用自己的线程池。

```xml
    
// name : 线程池名称
// namePrefix : 线程名称的前缀
// maxThreads ：最大线程
// minSpareThreads ：活跃线程数，（即使没有要执行的任务也不会销毁）
// maxIdleTime : 最大空闲时长 （线程空闲了指定时间后自动销毁）
// maxQueueSize : “Integer.MAX_VALUE” 在被执行前最大线程排队数量，默认为无限大，更改之后可能会有请求不被处理的情况发生。
// prestartminSpareThreads : 启动线程池时是否启动 minSpareThreads 的线程（核心线程，不会被销毁的线程），默认为 false
// threadPriority : 线程池中线程的优先级，1-10，默认为5
// className ： 线程实现类的名称，不指定的情况下会使用 org.apache.catalina.core.StandardThreadExecutor. 如果需要自定义，则自定义的线程池实现类需要实现org.apache.catalina.Executor 接口
<Executor 
          name="tomcatThreadPool" 
          namePrefix="catalina-exec-"
          maxThreads="150" 
          minSpareThreads="4"
          />
```

##### Connector 标签

Connector 用于创建连接器的实例，默认情况下 server.xml 配置了两个 Connector ，一个支持 HTTP 协议，一个支持 AJP 协议，

```xml
// port : 端口号，用于创建服务端 Socket 并进行监听，如果设置为 0，则会随机选择一个端口。
// protocol ：当前连接器 Connector 支持的访问协议，默认为 HTTP/1.1
// connectionTimeout ：链接超时时间
// redirectPort ：重定向端口号（https端口号）（假如接收到的是https协议，会直接将请求重定向到该端口）
// executor="tomcatThreadPool" ：指定共享线程池名称
// URIEncoding : 指定编码URI的字符编码
<Connector port="8081" 
           protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443" />

<Connector protocol="AJP/1.3"
           address="::1"
           port="8009"
           redirectPort="8443" />

// 可以直接指定指定连接器
protocol="org.apache.coyote.http11.Http11NioProtocol" 非阻塞式 Java NIO 连接器
protocol="org.apache.coyote.http11.Http11Nio2Protocol" 非阻塞式 Java NIO2 连接器
protocol="org.apache.coyote.http11.Http11AprProtocol" APR 连接器

protocol="org.apache.coyote.ajp.ajpNioProtocol"
protocol="org.apache.coyote.ajp.ajpNio2Protocol"
protocol="org.apache.coyote.ajp.ajpAprProtocol"
```



##### Engine 引擎

```xml

// name : 名称
// defaultHost ： 默认的 Host,这里指定的是 Host 的name
<Engine name="Catalina" defaultHost="localhost">

    <Realm className="org.apache.catalina.realm.LockOutRealm">
        <Realm />
    </Realm>

    <Host name="localhost"  appBase="webapps">
        <Valve  />
    </Host>
    <Host name="localhost2"  appBase="webapps">
        <Valve  />
    </Host>
</Engine>
```



###### Host 主机

一个 Engine 中可以有多个Host，但是只有一个是在 Engine 指定的默认 Host

==只要指定 Host 的名称，并且访问该名称 + Connector 指定协议的端口号，就可以找到该主机==

```xml
// name : 主机名
// appBase : 该主机部署应用所在目录，也就是当前 Host 部署的应用都应该放在该目录下
// unpackWARs： true 时，Host 启动时会将 webapps 目录下 war 包解压到当前目录。 false 时，Host 直接从 war 文件启动
// autoDeploy ： 是否自动定期检测并部署新增或者变更的 web 应用信息。
<Host name="localhost"  appBase="webapps"
      unpackWARs="true" autoDeploy="true">

    <context docBase="myApp" path="/myApp">
    </context>
    <!-- SingleSignOn valve, share authentication between web applications
             Documentation at: /docs/config/valve.html -->
    <!--
        <Valve className="org.apache.catalina.authenticator.SingleSignOn" />
        -->

    <!-- Access log processes all example.
             Documentation at: /docs/config/valve.html
             Note: The pattern used is equivalent to using pattern="common" -->
    <Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
           prefix="localhost_access_log" suffix=".txt"
           pattern="%h %l %u %t &quot;%r&quot; %s %b" />

</Host>
```

###### context 用于配置部署一个 web 应用

==例如`http://localhost:8080/myApp`简单来说就是新增了一级路径，相当于一个host 可以部署多个 web应用==

```xml
// docBase : Web 应用目录或者 war 包的部署路径，可以是绝对路径，也可以是相对于 Host appBase 的相对路径
// path : web 应用的 context 路径，例如 http://localhost:8080/myApp
<context docBase="myApp" path="/myApp">
    
</context>
```

