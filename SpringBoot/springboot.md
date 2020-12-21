# SpringBoot



[toc]



##### [spring boot官网](https://spring.io/projects/spring-boot)

#### 简介

springboot 为搭建程序的脚手架，可以快速地构建spring 项目。减少xml配置（约定大于配置）

### 搭建项目

#### 快速构建方式

1.  创建一个空项目

   New Project -> Empty Project

2.  创建模块

   New Module ->Spring Initializr

3. 选则要集成的东西，完成快速搭建

#### 手动搭建

1.  创建空项目

2. 创建maven module

3. 编写springboot 项目格式

   1. pom.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <modelVersion>4.0.0</modelVersion>
   
       <groupId>org.example</groupId>
       <artifactId>project-demo01</artifactId>
       <version>1.0-SNAPSHOT</version>
   
       <!--父工程，继承这里的依赖和版本号    -->
       <parent>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-parent</artifactId>
           <version>2.2.4.RELEASE</version>
       </parent>
   
       <!--java版本    -->
       <properties>
           <java.version>1.8</java.version>
       </properties>
   
       <!--依赖    -->
       <dependencies>
           <!--web 起步依赖        -->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
           </dependency>
       </dependencies>
   
   </project>
   ```

   2.  resources 下的 `application.yml`配置文件
   3.  java 下的 `com.example.Application.java`  引导类

   ```java
   package com.example;
   
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   
   @SpringBootApplication
   public class Demo01Application {
       public static void main(String[] args) {
   
           SpringApplication.run(Demo01Application.class,args);
       }
   }
   
   ```


#### 解析

###### @SpringBootApplication

注解内容为

```java
//
// Source code recreated from a .class file by IntelliJ IDEA
// (powered by FernFlower decompiler)
//

package org.springframework.boot.autoconfigure;

import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Inherited;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
import org.springframework.boot.SpringBootConfiguration;
import org.springframework.boot.context.TypeExcludeFilter;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.FilterType;
import org.springframework.context.annotation.ComponentScan.Filter;
import org.springframework.core.annotation.AliasFor;

@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
public @interface SpringBootApplication {
    @AliasFor(
        annotation = EnableAutoConfiguration.class
    )
    Class<?>[] exclude() default {};

    @AliasFor(
        annotation = EnableAutoConfiguration.class
    )
    String[] excludeName() default {};

    @AliasFor(
        annotation = ComponentScan.class,
        attribute = "basePackages"
    )
    String[] scanBasePackages() default {};

    @AliasFor(
        annotation = ComponentScan.class,
        attribute = "basePackageClasses"
    )
    Class<?>[] scanBasePackageClasses() default {};

    @AliasFor(
        annotation = Configuration.class
    )
    boolean proxyBeanMethods() default true;
}

```

主要集成了三个注解

- @SpringBootConfiguration

继承了`@Configuration`注解的功能。

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Configuration
public @interface SpringBootConfiguration {
    @AliasFor(
        annotation = Configuration.class
    )
    boolean proxyBeanMethods() default true;
}
```

- @EnableAutoConfiguration

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@AutoConfigurationPackage
@Import({AutoConfigurationImportSelector.class})
public @interface EnableAutoConfiguration {
    String ENABLED_OVERRIDE_PROPERTY = "spring.boot.enableautoconfiguration";

    Class<?>[] exclude() default {};

    String[] excludeName() default {};
}
```

主要为`@Import({AutoConfigurationImportSelector.class})`注解。将会加载内置配置文件，并将配置文件指定的对象加入到spring容器中

- @ComponentScan( excludeFilters = {

  ​	@Filter(type = FilterType.CUSTOM,classes = {TypeExcludeFilter.class}), 

  ​	@Filter( type = FilterType.CUSTOM, classes ={AutoConfigurationExcludeFilter.class})

  ​	}

  )

作用：定义扫描的路径从中找出标识了需要装配的类自动装配到spring的bean容器中（指定扫描的包，如果没有指定，则扫描此路径下的包）

```java
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.TYPE})
@Documented
@Repeatable(ComponentScans.class)
public @interface ComponentScan {
    
}

// 定义扫描的路径从中找出标识了需要装配的类自动装配到spring的bean容器中
```

###### spring-boot-starter-parent

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.2.4.RELEASE</version>
</parent>
```

- `spring-boot-starter-parent`继承了 `spring-boot-dependencies`

- `spring-boot-dependencies` 中定义了许多依赖和版本号，继承了`spring-boot-starter-parent` 就指定了那些依赖的版本号，用于解决版本冲突问题。

```xml
  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.2.4.RELEASE</version>
    <relativePath>../../spring-boot-dependencies</relativePath>
  </parent>
```

###### spring-boot-starter-web

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
```

- `spring-boot-starter-web`中引入了下面这些web必须的依赖

```xml
  <dependencies>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter</artifactId>
      <version>2.2.4.RELEASE</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-json</artifactId>
      <version>2.2.4.RELEASE</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-tomcat</artifactId>
      <version>2.2.4.RELEASE</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-validation</artifactId>
      <version>2.2.4.RELEASE</version>
      <scope>compile</scope>
      <exclusions>
        <exclusion>
          <artifactId>tomcat-embed-el</artifactId>
          <groupId>org.apache.tomcat.embed</groupId>
        </exclusion>
      </exclusions>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-web</artifactId>
      <version>5.2.3.RELEASE</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.2.3.RELEASE</version>
      <scope>compile</scope>
    </dependency>
  </dependencies>
```

###### @RestController

注解继承了

- `@Controller`(controller 层标识) 
- `@ResponseBody`（接口返回json格式数据）。

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Controller
@ResponseBody
public @interface RestController {
    @AliasFor(
        annotation = Controller.class
    )
    String value() default "";
}
```

###### @GetMapping

`@GetMapping` 继承了

- `@RequestMapping(method = {RequestMethod.GET})` （请求方式为get的路径）

```java
@Target({ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@RequestMapping(
    method = {RequestMethod.GET}
)
public @interface GetMapping {
    
}
```

###### 配置文件  yaml/yml

springboot支持yml、yaml、properties格式的配置文件

- yml，一种语言语言格式，能够被更多的语言通用。

- properties，java使用的，不通用

- 配置文件加载顺序，yaml->yml->properties。后加载的配置文件会覆盖先加载的配置

- yml 语法


```yml
  # 变量
  username: user
  
  # 数组
  address:
  	- 北京  # - 代表数组中的值
  	- 上海
  
  # 对象 map格式
  user:
  	name: name
  	age: age
  
  # 引用
  user: ${username} # 引用属性的值
```


- java 读取 yml 配置文件内容

```java
// 1. @Value

public class HelloController {
    // 单值
    @Value("${name}")
    private String name;
    // 数组
    @Value("${name[5]}")
    private String name;
    // 对象
    @Value("${user.name}")
    private String name;
}

// 2. Environment

public class HelloController {
	// 注入 Environment （import org.springframework.core.env.Environment;）
    @Autowired
    private Environment environment;

    @RequestMapping("1")
    public String hello(){
		
        // 获取值
        environment.getProperty("name");
        environment.getProperty("name[5]");
        environment.getProperty("user.name");
    }
}

// 3. @ConfigurationProperties 将对象属性值加载到配置类上

// 在配置类上加此注解，prefix（前缀）为对象名
@ConfigurationProperties(
    prefix = "server",
    ignoreUnknownFields = true
)
@Component // 声明配置类
@Data // 需要有getter setter 方法
public class ServerProperties {
    
    // 属性名和对象名一致，这里将直接获取值
    private Integer port;
    private InetAddress address;
}
```





#### springboot项目结构

###### 一、主目录：

```java
  src/main/java  程序开发以及主程序入口
  src/main/resources 配置文件
  src/test/java  测试程序
```

###### 二、结构：

```java
java
    com
        example
          myproject
            Application.java  启动类
            domain            实体类
              Customer.java
            service           service
              CustomerService.java
            controller        controller
              CustomerController.java
resources
    application.yml
    mappers
    	UserMapper.xml
```

#### springboot整合其他框架

##### springboot+mybatis

###### 1、依赖

```xml
<!--        事务-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>
<!--        mysql 链接-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
<!--        mybatis mybatis 提供的jar-->
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.1.1</version>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <scope>provided</scope>
        </dependency>
```

###### 2、yml 配置文件

```yml
# 服务端口
server:
  port: 8081
# 数据库
spring:
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql:///springbootdb
    username: root
    password: root

# mybatis
mybatis:
  type-aliases-package: com.example.entity # 实体类别名
  mapper-locations: classpath:/mappers/*.xml # mapper 文件位置

# 日志配置
logging:
  level: 
    com.example.dao: debug
```

3、@MapperScan(value="com.qianji.api.dao")

```java
@SpringBootApplication
// 对dao 接口进行动态代理生成对象
@MapperScan(value = "com.example.dao")
public class Demo01Application {
    public static void main(String[] args) {

        SpringApplication.run(Demo01Application.class,args);
    }
}
```



##### springboot+mybatis plus

###### 1.依赖

```xml
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.2.0</version>
        </dependency>
```

###### 2.DAO 继承BaseMapper继承默认增删改查方法

```java
package com.example.dao;

import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.example.entity.UserEntity;
// 继承BaseMapper<操作的实体类entity>
public interface UserDao extends BaseMapper<UserEntity> {
}
```

###### 3.启动类添加包扫描

```java
package com.example;

import org.mybatis.spring.annotation.MapperScan;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
// 对dao 接口进行动态代理生成对象
@MapperScan(value = "com.example.dao")
public class Demo01Application {
    public static void main(String[] args) {

        SpringApplication.run(Demo01Application.class,args);
    }
}
```

###### 4.Entity 配置映射数据库

```java
package com.example.entity;

import com.baomidou.mybatisplus.annotation.TableField;
import com.baomidou.mybatisplus.annotation.TableId;
import com.baomidou.mybatisplus.annotation.TableName;

// 表名
@TableName(value = "tb_user")
public class UserEntity {

    // 主键
    @TableId()
    private Long id;
    // 字段属性
    @TableField
    private String name;
    private int age;
}
```

##### springboot+thymeleaf

###### 1.依赖

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
```

###### 2.创建放置页面的文件夹

```java
resources/templates // 放置html页面

resources/static 	// 放置静态资源
```





##### springboot单元测试

###### 1、依赖

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
        </dependency>
```

###### 2、添加测试类

放在xxxApplication启动类同目录包下，或者添加 @SpringBootTest(class = xxxApplication) 才能执行容器注入等。


```java
  @RunWith(SpringRunner.class)
  @SpringBootTest(class = xxxApplication) //启动类
  public class HelloWorldControlerTests{
  
    @Test
    public void getHello(){
    
    }
  }
```

七、自定义filter

```java
  1.编写配置类，实现Filter 中的方法
  2.添加 @Configuration 注解
```

八、读取配置文件中的参数

```java
  自定义配置类
  @Component   注入ioc
  @ConfigurationProperties(prefix = "demo")    读取配置文件数据，prefix 指定前缀
  @PropertySource(value = "config.properties")    当配置文件不是application.yml 时，指定配置文件名。
  public class ConfigBeanProp {

      private String phone;     参数名对应字段名

      private String wife;
  }
```

九、默认日志

```properties
  logging.path=/user/local/log       日志打印路径
  logging.file=spring.log            日志文件名 （默认）
  logging.level.com.favorites=DEBUG  日志级别 logging.level
  logging.level.org.springframework.web=INFO   
  logging.level.org.hibernate=ERROR   
  logging.pattern.console=%d{yyyy/MM/dd-HH:mm:ss} [%thread] %-5level %logger- %msg%n    自定义日志格式
  logging.pattern.file=%d{yyyy/MM/dd-HH:mm} [%thread] %-5level %logger- %msg%n          
```

十、整合jpa

```java
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-jpa</artifactId>
  </dependency>
  
  # mysql 配置
  spring:
    datasource:
      driver-class-name: com.mysql.jdbc.Driver
      url: jdbc:mysql://192.168.1.20:3306/test
      username: root
      password: root
      type： com.alibaba.druid.pool.DruidDataSource
      
  spring.jpa.properties.hibernate.hbm2ddl.auto=update
  spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialect
  spring.jpa.show-sql= true
```

十一、整合 mybatisplus

      <!-- druid数据库连接池 -->
      <dependency>
          <groupId>com.alibaba</groupId>
          <artifactId>druid</artifactId>
          <version>${druid.version}</version>
      </dependency>
    
      <!-- mysql connector -->
      <dependency>
          <groupId>mysql</groupId>
          <artifactId>mysql-connector-java</artifactId>
          <scope>runtime</scope>
      </dependency>
    
      <!-- Mybatis-plus -->
      <dependency>
          <groupId>com.baomidou</groupId>
          <artifactId>mybatisplus-spring-boot-starter</artifactId>
          <version>${mybatisplus-spring-boot-starter.version}</version>
      </dependency>
      <dependency>
          <groupId>com.baomidou</groupId>
          <artifactId>mybatis-plus</artifactId>
          <version>${mybatis-plus.version}</version>
      </dependency>
    
      # mysql 配置
      spring:
        datasource:
          driver-class-name: com.mysql.jdbc.Driver
          url: jdbc:mysql://192.168.1.20:3306/test
          username: root
          password: root
          type： #数据连接池
    
      mybatis-plus:
        # xml扫描，多个目录用逗号或者分号分隔（告诉 Mapper 所对应的 XML 文件位置）
        mapper-locations: classpath:mapper/*.xml
        # 以下配置均有默认值,可以不设置
        global-config:
          db-config:
            #主键类型  auto:"数据库ID自增" 1:"用户输入ID",2:"全局唯一ID (数字类型唯一ID)", 3:"全局唯一ID UUID";
            id-type: auto
            #字段策略 IGNORED:"忽略判断"  NOT_NULL:"非 NULL 判断")  NOT_EMPTY:"非空判断"
            field-strategy: NOT_EMPTY
            #数据库类型
            db-type: MYSQL
        configuration:
          # 是否开启自动驼峰命名规则映射:从数据库列名到Java属性驼峰命名的类似映射
          map-underscore-to-camel-case: true
          # 如果查询结果中包含空值的列，则 MyBatis 在映射的时候，不会映射这个字段
          call-setters-on-nulls: true
          # 这个配置会将执行的sql打印出来，在开发或测试的时候可以用
          log-impl: org.apache.ibatis.logging.stdout.StdOutImpl


        启动类添加注解扫描dao 
        @SpringBootApplication
        @MapperScan(basePackages = {"com.mp.demo.dao"}) //扫描DAO
        
        配置类
        /**
         * @Description MybatisPlus配置类
         */
        @Configuration
        public class MybatisPlusConfig {
            /**
             * mybatis-plus SQL执行效率插件【生产环境可以关闭】
             */
            @Bean
            public PerformanceInterceptor performanceInterceptor() {
                return new PerformanceInterceptor();
            }
            /**
             * 分页插件
             */
            @Bean
            public PaginationInterceptor paginationInterceptor() {
                return new PaginationInterceptor();
            }
        }
        
        Entity 类
        @TableName("user_info")    //@TableName中的值对应着表名
        public class UserInfoEntity {
    
            /**
             * 主键
             * @TableId中可以决定主键的类型,不写会采取默认值,默认值可以在yml中配置
             * AUTO: 数据库ID自增
             * INPUT: 用户输入ID
             * ID_WORKER: 全局唯一ID，Long类型的主键
             * ID_WORKER_STR: 字符串全局唯一ID
             * UUID: 全局唯一ID，UUID类型的主键
             * NONE: 该类型为未设置主键类型
             */
            @TableId(type = IdType.AUTO)
            private Long id;
    
        //dao 
        public interface UserInfoDao extends BaseMapper<UserInfoEntity> {
        }
        
        //service
        public interface UserInfoService extends IService<UserInfoEntity> {
        }
        
        //serviceimpl
        @Service
        @Transactional
        public class UserInfoSerivceImpl extends ServiceImpl<UserInfoDao, UserInfoEntity> implements UserInfoService {
        }


        setSqlSelect	  设置 SELECT 查询字段
        where	          WHERE 语句，拼接 + WHERE 条件
        and	            AND 语句，拼接 + AND 字段=值
        or	            OR 语句，拼接 + OR 字段=值
        eq	            等于=
        allEq	          基于 map 内容等于=
        ne	            不等于<>
        gt	            大于>
        ge	            大于等于>=
        lt	            小于<
        le	            小于等于<=
        like	          模糊查询 LIKE
        notLike	        模糊查询 NOT LIKE
        in	            IN 查询
        notIn	          NOT IN 查询
        isNull	        NULL 值查询
        isNotNull	      IS NOT NULL
        groupBy	        分组 GROUP BY
        having	        HAVING 关键词
        orderBy	        排序 ORDER BY
        orderByAsc	    ASC 排序 ORDER BY
        orderByDesc	    DESC 排序 ORDER BY
        exists	        EXISTS 条件语句
        notExists	      NOT EXISTS 条件语句
        between	        BETWEEN 条件语句
        notBetween	    NOT BETWEEN 条件语句
        addFilter	      自由拼接 SQL
        last	          拼接在最后，例如：last("LIMIT 1")

十二、整合 Redis

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>
    
        spring:
            redis:
              host: 127.0.0.1 
              port: 6379
              password: 123456
              jedis:
                pool:
                  max-active: 8
                  max-wait: -1
                  max-idle: 500
                  min-idle: 0
              lettuce:
                shutdown-timeout: 0




​      