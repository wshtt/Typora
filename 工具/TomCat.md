# tomcat

1. 目录

        bin：可以执行文件。
        conf：tomcat服务器的配置文件
        lib：tomcat启动后需要依赖的jar包
        logs：tomcat工作之后的日志文件
        webapps：是tomcat布暑工程的目录。
        work：jsp文件在被翻译之后，保存在当前这个目录下，session对象被序列化之后保存的位置
    
2. 启动

        1. 双击/bin/startup.bat
        2. 在命令行安装目录bin下，执行catalina run，会提示启动信息。

3. 乱码

        日志打印乱码，在conf/logging.properties中，更改显示编码为本地电脑显示编码
        java.util.logging.ConsoleHandler.encoding = UTF-8
        修改为
        java.util.logging.ConsoleHandler.encoding = GBK

4. 版本 tomcat 依赖于java版本，64与32混用会报错

5. 端口修改

        conf/server.xml 中
        
        <Connector port="8080" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443" />
           
        此处修改端口 
    
6. 部署项目

        1. jsp+servlet项目
            在conf/server.xml中的host内配置
            <Context  path="/atguigu"  docBase="D:\atguigu"/>
            <Context  path=”浏览器要访问的目录---虚拟目录”  docBase=”网站所在磁盘目录”/>
            
        2. 将文件放在webapps目录下，名称为访问的目录
            相当于前面的指定了 名称和位置
            
        3. 将项目打成war包放在webapps目录下，名称为项目名。
    
        4. 直接通过ip+端口访问的是/webapps/ROOT目录项目，如果有一个叫ROOT的项目，则直接通过ip+端口访问



7. xml 解析  优先级：.m2文件下为局部配置>conf文件下的全局配置

        1. localRepository  配置本地仓库
          默认：${user.home}  指的是 user/用户名/  
          <localRepository>${user.home}/.m2/repository</localRepository>
          修改：
          <localRepository>D:\maven-repository\repository</localRepository>
          
        2. 