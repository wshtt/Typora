# MongoDB

1.  "show dbs" 命令可以显示所有数据的列表。
2.  执行 "db" 命令可以显示当前数据库对象或集合。
3.  运行"use db"命令，可以连接到一个指定的数据库。


$数据库名$
    不能是空字符串
    不得含有'' 空格 . $ 、/、\0
    应该全部小写
    最多64字节
    
$$



### 问题

###### mongoTemplate 保存问题

```java
mongoTemplate  自动把所有bean（包括内嵌的bean）的id 转成了_id，save操作也是如此。
```



