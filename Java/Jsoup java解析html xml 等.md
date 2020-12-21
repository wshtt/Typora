# Jsoup



**jsoup是一款Java的HTML解析器，主要用来对HTML解析。**

**可以解析一系列标签结构的文件**（xml，html，xhtml等等）

#### 一 、依赖

```xml
<dependency>
    <groupId>org.jsoup</groupId>
    <artifactId>jsoup</artifactId>
    <version>1.11.3</version>
</dependency>
```

#### 二、获取Document 对象

```java
// 1. 通过http https链接
Connection connect = Jsoup.connect("http://www.baidu.com/");
Document document = connect.get();

// org.jsoup.Connection 对象可以设置一些请求头之类的信息来模拟浏览器的行为
connect.data("query", "Java")			// 数据
    .userAgent("Mozilla")				//
    .cookie("auth", "token")			// cookie
    .timeout(3000)						//超时时间
    .cookies(Map<String,String>)		// cookies
    .header("请求头","")				  // 请求头
    .headers(Map<String,String>);		//

Document document = connect.get();
   .post();							// 请求方法，get post

// 2. 通过字符串的html文件
Document sss = Jsoup.parse("sss");

// 3. 通过一部分html 字符串，比如一个标签，这样会把所有
Document sss = Jsoup.parseBodyFragment("sss");
```

