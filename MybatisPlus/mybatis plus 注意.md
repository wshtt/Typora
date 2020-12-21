# MyBatis Plus注意

1. mybatis-plus 默认不会保存/更换新空内容进数据库。如果要将字段设置为null，需要在实体类添加 `@TableField(strategy=FieldStrategy.NOT_EMPTY)`

2. 默认主键名为 id ，可以通过 @TableId 指定主键

3. mybatisplus Wrapper的or使用
    //mybatisplus版本3.0.7.1 jdk1.8 
    
        queryWrapper.and(wrapper -> wrapper.eq("pid", bid).or().eq("pid2", bid));
        //执行sql
        select * form business where deleted=0 and (pid=1 or pid2=1)

    //mybatisplus版本低于3.0.7.1
    
        queryWrapperw.andNew().eq("pid", bid).or().eq("pid2", bid);
        //执行sql
        select * form business where deleted=0 and (pid=1 or pid2=1)
    
4. mapper 配置文件配置mapper 存放位置
        mybatis-plus:
          mapper-locations:
            - com/mp/mapper/*



5. 分页 page

          参数 page querywrapper
        IPage<T> selectPage(IPage<T> page, @Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
        IPage<Map<String, Object>> selectMapsPage(IPage<T> page, @Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
      
        new Page(1,10,false) 当前页/每页条数/是否查询总记录数。（false则为查询的total为空，少一条sql）
      
6. 自定义sql分页

          dao层写自定义分页接口
        IPage<Map<String, Object>> selectMyPage(IPage<T> page, @Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
      
        接口对应的sql
        select * from user ${ew.customSqlSegment}
      
7. 主键策略    默认雪花算法

    ```java
    /**
     * 数据库ID自增   （数据库设置primary key auto_increment）
     */
    AUTO(0),
    /**
     * 该类型为未设置主键类型     跟随全局策略
     */
    NONE(1),
    /**
     * 用户输入ID
     * <p>该类型可以通过自己注册自动填充插件进行填充</p>   用户自己填充id
     */
    INPUT(2),
    
    /* 以下3种类型、只有当插入对象ID 为空，才自动填充。 */  自己未填充id，则自动填充
    /**
     * 全局唯一ID (idWorker)
     */
    ID_WORKER(3),
    /**
     * 全局唯一ID (UUID)
     */
    UUID(4),
    /**
     * 字符串全局唯一ID (idWorker 的字符串表示)
     */
    ID_WORKER_STR(5);
    ```
    
    
    ​    
    ​    全局id策略
      mybatis-plus:
       global-config:
    ​    id-type: uuid
    ​    
    
8. Active Record 模式  直接通过对象调用

  实体类继承Model<Entity>

  实体类添加`serialVersionUID`
  private static final long serialVersionUID = -3112677461957141934L;

  实体类添加 `@EqualsAndHashCode`  // 不加也行,会有warn

  Mapper 继承 `extends BaseMapper<Entity>`

  使用

        UserPo userPo = new UserPo();
        userPo.setId(36);
        // 通过设置实体上的数据查询
        UserPo userPo1 = userPo.selectById();
        logger.info("selectById1:"+userPo1);
        // 通过直接传参查询
        UserPo userPo2 = userPo.selectById(36);
        logger.info("selectById2:"+userPo2);
        // 通过构造wrapper查询
        UserPo userPo3 = userPo.selectOne(new QueryWrapper<UserPo>().lambda().eq(UserPo::getId, 36));
        logger.info("selectOne3:"+userPo3);


9. 注解`@MapperScan`
    在启动类上添加此注解指定mapper包，这样就不需要在每个mapper接口添加`@mapper`注解
    
10. 配置 
    
    1.  config-Location  指定mybatis主配置文件位置
    2.  mapper-Locations  指定mapper.xml文件位置   Maven 多模块项目的扫描路径需以 classpath*: 开头 （即加载多个 jar 包下的 XML 文件）
    3.  type-AliasesPackage 别名包路径配置  注册后在 Mapper 对应的 XML 文件中可以直接使用类名，而不用使用全限定的类名
    4.  
    