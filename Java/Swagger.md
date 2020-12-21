# Swagger
Swagger 是最流行的 API 开发工具，它遵循 OpenAPI Specification（OpenAPI 规范，也简称 OAS）。
Swagger 可以贯穿于整个 API 生态，如 API 的设计、编写 API 文档、测试和部署。
Swagger 是一种通用的，和编程语言无关的 API 描述规范。

应用场景
1.  如果你的 RESTful API 接口都开发完成了，你可以用 Swagger-editor 来编写 API 文档（ yaml 文件 或 json 文件），然后通过 Swagger-ui 来渲染该文件，以非常美观的形式将你的 API 文档，展现给你的团队或者客户。
2.  如果你的 RESTful API 还未开始，也可以使用 Swagger ，来设计和规范你的 API，以 Annotation （注解）的方式给你的源代码添加额外的数据。这样，Swagger 就可以检测到这些数据，自动生成对应的 API 文档。

  Swagger 规范本身是与编程语言无关的，它支持两种语法风格：

    YAML 语法
    JSON 语法

【springboot集成swagger-ui自动生成API文档】
1.添加依赖

          <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger2</artifactId>
            <version>2.2.2</version>
          </dependency>
    
          <dependency>
              <groupId>io.springfox</groupId>
              <artifactId>springfox-swagger-ui</artifactId>
              <version>2.2.2</version>
          </dependency>


2.编写配置文件
在application同级目录新建swagger2文件，添加swagger2配置类

        package com.abel.example;
        import org.springframework.context.annotation.Bean;
        import org.springframework.context.annotation.Configuration;
        import springfox.documentation.builders.ApiInfoBuilder;
        import springfox.documentation.builders.PathSelectors;
        import springfox.documentation.builders.RequestHandlerSelectors;
        import springfox.documentation.service.ApiInfo;
        import springfox.documentation.spi.DocumentationType;
        import springfox.documentation.spring.web.plugins.Docket;
        import springfox.documentation.swagger2.annotations.EnableSwagger2;
    
        @Configuration
        @EnableSwagger2
        public class Swagger2 {
            /**
             * 创建API应用
             * apiInfo() 增加API相关信息
             * 通过select()函数返回一个ApiSelectorBuilder实例,用来控制哪些接口暴露给Swagger来展现，
             * 本例采用指定扫描的包路径来定义指定要建立API的目录。
             *
             * @return
             */
            @Bean
            public Docket createRestApi() {
                return new Docket(DocumentationType.SWAGGER_2)
                        .apiInfo(apiInfo())
                        .select()
                        //加了ApiOperation注解的类，才生成接口文档
                        //.apis(RequestHandlerSelectors.withMethodAnnotation(ApiOperation.class))
                        //包下的类，才生成接口文档
                        .apis(RequestHandlerSelectors.basePackage("com.qianji.controller"))
                        .paths(PathSelectors.any())
                        .build()
                        .directModelSubstitute(java.util.Date.class, String.class)
                        .securitySchemes(security());
            }
    
            /**
             * 创建该API的基本信息（这些基本信息会展现在文档页面中）
             * 访问地址：http://项目实际地址/swagger-ui.html
             * @return
             */
            private ApiInfo apiInfo() {
                return new ApiInfoBuilder()
                        .title("Spring Boot中使用Swagger2构建RESTful APIs")
                        .description("更多请关注https://blog.csdn.net/u012373815")
                        .termsOfServiceUrl("https://blog.csdn.net/u012373815")
                        .contact("abel")
                        .version("1.0")
                        .build();
            }
        }

3.在controller上添加注解，自动生成API

        package com.abel.example.controller;
        import javax.servlet.http.HttpServletRequest;
        import java.util.Map;
        import com.abel.example.bean.User;
        import io.swagger.annotations.*;
        import org.apache.log4j.Logger;
        import org.springframework.beans.factory.annotation.Autowired;
        import org.springframework.http.HttpStatus;
        import org.springframework.http.ResponseEntity;
        import org.springframework.stereotype.Controller;
        import org.springframework.web.bind.annotation.*;
        import com.abel.example.service.UserService;
        import com.abel.example.util.CommonUtil;
    
        @Controller
        @RequestMapping(value = "/users")
        @Api(value = "用户的增删改查")
        public class UserController {
    
            @Autowired
            private UserService userService;


            /**
             * 查询所有的用户
             * api :localhost:8099/users
             * @return
             */
            @RequestMapping(method = RequestMethod.GET)
            @ResponseBody
            @ApiOperation(value = "获取用户列表，目前没有分页")
            public ResponseEntity<Object> findAll() {
                return new ResponseEntity<>(userService.listUsers(), HttpStatus.OK);
            }
    
            /**
             * 通过id 查找用户
             * api :localhost:8099/users/1
             * @param id
             * @return
             */
            @RequestMapping(value = "/{id}", method = RequestMethod.GET)
            @ResponseBody
            @ApiOperation(value = "通过id获取用户信息", notes="返回用户信息")
            public ResponseEntity<Object> getUserById(@PathVariable Integer id) {
                return new ResponseEntity<>(userService.getUserById(Long.valueOf(id)), HttpStatus.OK);
            }


            /**
             * 通过spring data jpa 调用方法
             * api :localhost:8099/users/byname?username=xxx
             * 通过用户名查找用户
             * @param request
             * @return
             */
            @RequestMapping(value = "/byname", method = RequestMethod.GET)
            @ResponseBody
            @ApiImplicitParam(paramType = "query",name= "username" ,value = "用户名",dataType = "string")
            @ApiOperation(value = "通过用户名获取用户信息", notes="返回用户信息")
            public ResponseEntity<Object> getUserByUserName(HttpServletRequest request) {
                Map<String, Object> map = CommonUtil.getParameterMap(request);
                String username = (String) map.get("username");
                return new ResponseEntity<>(userService.getUserByUserName(username), HttpStatus.OK);
            }
    
            /**
             * 通过spring data jpa 调用方法
             * api :localhost:8099/users/byUserNameContain?username=xxx
             * 通过用户名模糊查询
             * @param request
             * @return
             */
            @RequestMapping(value = "/byUserNameContain", method = RequestMethod.GET)
            @ResponseBody
            @ApiImplicitParam(paramType = "query",name= "username" ,value = "用户名",dataType = "string")
            @ApiOperation(value = "通过用户名模糊搜索用户信息", notes="返回用户信息")
            public ResponseEntity<Object> getUsers(HttpServletRequest request) {
                Map<String, Object> map = CommonUtil.getParameterMap(request);
                String username = (String) map.get("username");
                return new ResponseEntity<>(userService.getByUsernameContaining(username), HttpStatus.OK);
            }


            /**
             * 添加用户啊
             * api :localhost:8099/users
             *
             * @param user
             * @return
             */
            @RequestMapping(method = RequestMethod.POST)
            @ResponseBody
            @ApiModelProperty(value="user",notes = "用户信息的json串")
            @ApiOperation(value = "新增用户", notes="返回新增的用户信息")
            public ResponseEntity<Object> saveUser(@RequestBody User user) {
                return new ResponseEntity<>(userService.saveUser(user), HttpStatus.OK);
            }
    
            /**
             * 修改用户信息
             * api :localhost:8099/users
             * @param user
             * @return
             */
            @RequestMapping(method = RequestMethod.PUT)
            @ResponseBody
            @ApiModelProperty(value="user",notes = "修改后用户信息的json串")
            @ApiOperation(value = "新增用户", notes="返回新增的用户信息")
            public ResponseEntity<Object> updateUser(@RequestBody User user) {
                return new ResponseEntity<>(userService.updateUser(user), HttpStatus.OK);
            }
    
            /**
             * 通过ID删除用户
             * api :localhost:8099/users/2
             * @param id
             * @return
             */
            @RequestMapping(value = "/{id}", method = RequestMethod.DELETE)
            @ResponseBody
            @ApiOperation(value = "通过id删除用户信息", notes="返回删除状态1 成功 0 失败")
            public ResponseEntity<Object> deleteUser(@PathVariable Integer id) {
                return new ResponseEntity<>(userService.removeUser(id.longValue()), HttpStatus.OK);
            }
        }
4.下载Swagger UI组件
去官网下载Zip包，或者在github上下载也可以，需要将dist文件夹下的所有文件的复制到webapp目录下
原理就是在系统加载的时候，Swagger配置类去扫描所有添加注释的接口，并且储存起来通过下面地址进行访问，返回JSON数据，在前端界面显示出来。
启动项目后，访问http://localhost:8099/swagger-ui.html，显示如下

注解说明

@Api：用在类上，说明该类的作用。
@ApiOperation：注解来给API增加方法说明。
@ApiImplicitParams : 用在方法上包含一组参数说明。
@ApiImplicitParam：用来注解来给方法入参增加说明。
@ApiResponses：用于表示一组响应
@ApiResponse：用在@ApiResponses中，一般用于表达一个错误的响应信息
@ApiModel：描述一个Model的信息（一般用在请求参数无法使用@ApiImplicitParam注解进行描述的时候）
@ApiModelProperty：描述一个model的属性
其中
@ApiResponse参数：

code：数字，如400
message：信息，如“参数填写错误”
response：抛出异常的类
@ApiImplicitParam参数：
paramTpye：指定参数放在哪些地方（header/query/path/body/form）
name：参数名
dataTpye：参数类型
required：是否必输（true/false）
value：说明参数的意思
defaultValue：参数默认值


​    