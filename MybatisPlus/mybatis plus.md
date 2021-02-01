# Mybatis plus

介绍 ：在 mybatis 的基础上只做增强不做改变，简化开发，提高效率。

    1. 内置 CURD 操作
    2. 内置通用 Mapper、通用 Service
    3. 支持 Lambda 形式调用
    4. 支持主键自动生成（内置四种主键生成策略）内含分布式唯一 ID 生成器 - Sequence

mybatis plus 包括 
  core 核心
  generator 代码生成器
  annotation 注解
  extension 扩展
  mybatis-plus-boot-starter springboot 启动器


## 一 配置环境

pom.xml
    
```xml
<dependencies>
  //启动器
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter</artifactId>
  </dependency>
  //测试
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-test</artifactId>
      <scope>test</scope>
  </dependency>
  //lombok
  <dependency>
      <groupId>org.projectlombok</groupId>
      <artifactId>lombok</artifactId>
      <optional>true</optional>
  </dependency>
  //mybatis plus
  <dependency>
      <groupId>com.baomidou</groupId>
      <artifactId>mybatis-plus-boot-starter</artifactId>
      <version>3.3.1.tmp</version>
  </dependency>
  //mysql
  <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <scope>runtime</scope>
  </dependency>
</dependencies>
```

application.yml
    
```yaml
spting.datasource.driver-class-name:
spting.datasource.url:
spting.datasource.username:
spting.datasource.password:
```


dao

```markdown
1. 继承baseMapper<entity名称>
2. 在启动类添加MapperScan(Mapper路径)
```

entity

```java
1.@TaableName("表名") 对应数据库表名
2.@TableId      对应数据库的主键
  (1) type 主键生成类型
3.ableFieid("字段名")    对应数据库字段

4.实体类有数据库不存在字段的三种方案 (忽略插入此字段)
  (1) private transient String name;  这样不会被序列化
  (2) private static String name;    静态化
  (3) @TableFieid(exist=fase)   表明此字段数据库不存在
```


​      
查询

```java
1. selectByMap(Map<String,Object>)   数据库字段名，数据

2. Wrapper 的两种创建方式
      new  QueryWrapper
      Wrappers.<泛型>query()
  
3. like("name","张")  包含张
   likeRight("name","张")   姓张
   likeLeft"name","张")  以张结尾

4. or  直接调用or().   意思就是或者,or如果需要带括号，则需要使用lambda表达式的写法
    wrapper.and(w -> w.eq("id","1").or().eq("id","2"))
    or后面嵌套
    or(i -> i.eq("name", "李白").ne("status", "活着"))--->or (name = '李白' and status <> '活着')

5. orderByAsc("xxx").orderByDesc("vvv")  xxx升序，xxx相同则按vvv降序

6. apply 多用于sql函数，可以直接展示出来
      dateColumn字段等于2008-08-08（属于变相的eq）   
      apply("date_format(dateColumn,'%Y-%m-%d') = '2008-08-08'")--->date_format(dateColumn,'%Y-%m-%d') = '2008-08-08'")
      
      防止sql注入方法  {这里为后面参数的索引，从0开始}
      apply("date_format(dateColumn,'%Y-%m-%d') = {0}", "2008-08-08")--->date_format(dateColumn,'%Y-%m-%d') = '2008-08-08'")
      
7. in 两种用法   notIn 类似
      in("age", 1, 2, 3)--->age in (1,2,3)   单个值
      in("age",{1,2,3})--->age in (1,2,3)     集合

8. inSql   notInSql类似
      inSql("age", "1,2,3,4,5,6")--->age in (1,2,3,4,5,6)     字符串
      inSql("id", "select id from table where id < 3")--->id in (select id from table where id < 3)      SQL语句
      
9. having 
      having("sum(age) > 10")--->having sum(age) > 10
      having("sum(age) > {0}", 11)--->having sum(age) > 11     防止sql注入
      
10. and   里面使用lambda表达式，用来把sql放入括号内
      and(i -> i.eq("name", "李白").ne("status", "活着"))--->and (name = '李白' and status <> '活着') 
    
11. last
      只可以调用一次，拼接到末尾，有sql注入风险
      last("limit 1")

12. exists     拼接exists
      exists("select id from table where age = 1")--->exists (select id from table where age = 1)

13. nested  单纯的嵌套（括号）
      nested(i -> i.eq("name", "李白").ne("status", "活着"))--->(name = '李白' and status <> '活着')
      
14. allEq 
      allEq(Map<R, V> params, boolean null2IsNull)
      allEq(Map<R, V> params)
      params : key为数据库字段名,value为字段值
      null2IsNull : 为true则在map的value为null时调用 isNull 方法,为false时则忽略value为null的
```


简单查询      
          
    1. eq   =        等于
    2. ne   <>       不等于
    3. gt   >        大于
    4. ge   >=       大于等于
    5. lt   <        小于
    6. le   <=       小于等于
    7. between
    8. notBetween
    9. like
    10. isNull
    11. isNotNull
    12. groupBy      分组


​    
其他查询

```java
1. select
  查询指定字段
  select(String... sqlSelect)
  select("id","name","age")
  排除查询结果  class在queryWrapper有泛型则不需要
  select(Class<T> entityClass, Predicate<TableFieldInfo> predicate)
  select(i -> i.getProperty().startsWith("test"))
  select(i -> !i.getColnum().equals("age")&&!i.getColnum().equals("time"))
  
2. queryWrapper<entity>(user)
    这里的参数是作为查询条件的，与后面的eq等叠加，谨慎使用。
    在实体类这个字段添加注解来对条件进行大于小于模糊等查询
    @TableField(condition=SqlCondition.like)
    
3. lambda表达式使用
      三种方式
    LambdaQueryWrapper<String> wrapper1 = new LambdaQueryWrapper<>();
    LambdaQueryWrapper<String> wrapper2 = new QueryWrapper<String>().lambda();
    LambdaQueryWrapper<String> wrapper3 = Wrappers.lambdaQuery(); 
      使用
    wrapper1.like(User::getName,"雨").it(User::getAge,40);
    wrapper2.like(User::getName,"雨").and(w -> w.eq(User::getAge,40).or().isNotNull(User::getName));
```

### 问题

---
#### mybatis plus in （字符串数组）的问题


```java
// wrapper 使用 apply 拼接 sql 函数
String str = "";
str += "add_user";
str += ",";
str += "add_user_park";
str += ",";
str += "s";
wrapper.apply("FIND_IN_SET(appcode,+" str "+)");
wrapper.apply("FIND_IN_SET(appcode,{0})",str);
// {0} 代表第一个参数， 这种传参可以减少sql 注入问题
```
---
```SQL
# FIND_IN_SET(str,strlist)  
# str 指的是输入的字符
# strlist 指的是集合转换成的字符串
select FIND_IN_SET('b', 'a，b，c'); # 结果 2 存在则返回未知
select FIND_IN_SET('d', 'a，b，c'); # 结果 0 不存在返回0
select FIND_IN_SET('1', '1');

# 不能使用 in 的原因
# in() 接收常量字符串，接收数字类型变量与常量
```

