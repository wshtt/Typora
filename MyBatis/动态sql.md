# 动态SQL



[toc]



###### 简介

动态 SQL 是 MyBatis 的强大特性之一。

使用动态 SQL 并非一件易事，但借助可用于任何 SQL 映射语句中的强大的动态 SQL 语言，MyBatis 显著地提升了这一特性的易用性。

如果你之前用过 JSTL 或任何基于类 XML 语言的文本处理器，你对动态 SQL 元素可能会感觉似曾相识。在 MyBatis 之前的版本中，需要花时间了解大量的元素。借助功能强大的基于 OGNL 的表达式，MyBatis 3 替换了之前的大部分元素，大大精简了元素种类，现在要学习的元素种类比原来的一半还要少。

### 标签

##### `if`

`if`标签，如果`test`中的条件成立，则会将`if`内容拼接到SQL中。

```xml
<select id="findActiveBlogWithTitleLike" resultType="Blog">
  SELECT * FROM BLOG
  WHERE state = ‘ACTIVE’
  <if test="title != null and title ！= ''">
    AND title like #{title}
  </if>
</select>
```

```xml
<select id="findActiveBlogLike" resultType="Blog">
  SELECT * FROM BLOG 
  WHERE state = ‘ACTIVE’
  <if test="title != null">
    AND title like #{title}
  </if>
  <if test="author != null and author.name != null">
    AND author_name like #{author.name}
  </if>
</select>
```

##### `choose` `when` `otherwise`

选择条件中的一个，当`when`的条件成立，则拼接SQL,`choose`结束。如果两个`when`都不满足条件，则添加`otherwise`中的SQL

```xml
<select id="findActiveBlogLike" resultType="Blog">
  SELECT * FROM BLOG 
  WHERE state = ‘ACTIVE’
  <choose>
    <when test="title != null">
      AND title like #{title}
    </when>
    <when test="author != null and author.name != null">
      AND author_name like #{author.name}
    </when>
    <otherwise>
      AND featured = 1
    </otherwise>
  </choose>
</select>
```

##### `where`

当`if`都不成立时，不会拼接`where`，只有当`if`成立才会拼接`where`。如果`where`后面跟的语句是`and`或者`or`开头的，则会自动去掉，保证SQL正确。 

```xml
<select id="findActiveBlogLike"
     resultType="Blog">
  SELECT * FROM BLOG
  <where>
    <if test="state != null">
        AND state = #{state}
    </if>
    <if test="title != null">
        AND title like #{title}
    </if>
    <if test="author != null and author.name != null">
        AND author_name like #{author.name}
    </if>
  </where>
</select>
```

##### `trim`

mybatis的**trim**标签一般用于去除sql语句中多余的and关键字、逗号，或者给sql语句前拼接 “where“、“set“以及“values(“ 等前缀，或者添加“)“等后缀，可用于选择性插入、更新、删除或者条件查询等操作。

|      属性       |                             作用                             |
| :-------------: | :----------------------------------------------------------: |
|     prefix      |                     给sql语句拼接的前缀                      |
|     suffix      |                     给sql语句拼接的后缀                      |
| prefixOverrides | 去除sql语句前面的关键字或者字符，该关键字或者字符由prefixOverrides属性指定，假设该属性指定为"AND"，当sql语句的开头为"AND"，trim标签将会去除该"AND" |
| suffixOverrides | 去除sql语句后面的关键字或者字符，该关键字或者字符由suffixOverrides属性指定 |

```xml
<trim prefix="WHERE" prefixOverrides="AND">
	<if test="state != null">
	  state = #{state}
	</if> 
	<if test="title != null">
	  AND title like #{title}
	</if>
	<if test="author != null and author.name != null">
	  AND author_name like #{author.name}
	</if>
</trim>

前缀 WHERE 
连接词 AND

```

##### `set`

`set`标签会自动把`if`成立的拼接起来，不成立的不会拼接。如果都不成立则不拼接，会自动去掉结尾处的`,`

```xml
<update id="updateAuthorIfNecessary">
  update Author
    <set>
      <if test="username != null">
          username=#{username},
      </if>
      <if test="password != null">
          password=#{password},
      </if>
      <if test="email != null">
          email=#{email},
      </if>
      <if test="bio != null">
          bio=#{bio}
      </if>
    </set>
  where id=#{id}
</update>
```

##### `foreach`

*foreach* 元素的功能非常强大，它允许你指定一个集合，声明可以在元素体内使用的集合项（item）和索引（index）变量。

当使用可迭代对象或者数组时，index 是当前迭代的序号，item 的值是本次迭代获取到的元素。当使用 Map 对象（或者 Map.Entry 对象的集合）时，index 是键，item 是值。

- `item`:值
- `index`:索引
- `collection`:集合
- `open`:开头
- `separator`:分隔符
- `close`: 结尾

```xml
<select id="selectPostIn" resultType="domain.blog.Post">
  SELECT *
  FROM POST P
  WHERE ID in
  <foreach item="item" index="index" collection="list"
      open="(" separator="," close=")">
        #{item}
  </foreach>
</select>
```

##### 注解动态SQL

使用 *script* 元素包裹

```java
    @Update({"<script>",
      "update Author",
      "  <set>",
      "    <if test='username != null'>username=#{username},</if>",
      "    <if test='password != null'>password=#{password},</if>",
      "    <if test='email != null'>email=#{email},</if>",
      "    <if test='bio != null'>bio=#{bio}</if>",
      "  </set>",
      "where id=#{id}",
      "</script>"})
    void updateAuthorValues(Author author);
```

##### bind

`bind` 元素允许你在 OGNL 表达式以外创建一个变量，并将其绑定到当前的上下文。

即可以用 `#{}` 来使用定义的变量

```xml
<select id="selectBlogsLike" resultType="Blog">
  <bind name="pattern" value="'%' + _parameter.getTitle() + '%'" />
  SELECT * FROM BLOG
  WHERE title LIKE #{pattern}
</select>
```

##### 多数据源

........

```xml
<insert id="insert">
  <selectKey keyProperty="id" resultType="int" order="BEFORE">
    <if test="_databaseId == 'oracle'">
      select seq_users.nextval from dual
    </if>
    <if test="_databaseId == 'db2'">
      select nextval for seq_users from sysibm.sysdummy1"
    </if>
  </selectKey>
  insert into users values (#{id}, #{name})
</insert>
```

