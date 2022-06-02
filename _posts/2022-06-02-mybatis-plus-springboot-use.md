---
layout: post
title: "Mytabis-plus的简单使用指南及其在Spring Boot中的整合"
subtitle: "MyBatis-Plus是一个MyBatis的增强工具，在MyBatis的基础上只做增强不做改变，为简化开发、提高效率而生。"
date: 2020-09-20
author: "Garden"
header-img: "img/post-bg-2015.jpg"
tags: [SpringBoot, MybatisPlus, 数据库]
---
@[TOC](Mytabis-plus)
# 一、MybatisPlus简介
[MybatisPlus官网](http://mp.baomidou.com/)
[MybatisPlus教程](http://mp.baomidou.com/guide/)

**MyBatis-Plus(简称 MP)**是一个 MyBatis 的增强工具，在 MyBatis 的基础上只做增强不做改变，为简化开发、提高效率而生。
* **MyBatis-Plus特性**
1. **无侵入**：只做增强不做改变，引入它不会对现有工程产生影响，如丝般顺滑
2. **损耗小**：启动即会自动注入基本 CURD，性能基本无损耗，直接面向对象操作
3. **强大的 CRUD 操作**：内置通用 Mapper、通用 Service，仅仅通过少量配置即可实现单表大部分 CRUD 操作，更有强大的条件构造器，满足各类使用需求
4. **支持 Lambda 形式调用**：通过 Lambda 表达式，方便的编写各类查询条件，无需再担心字段写错
5. **支持多种数据库**：支持 MySQL、MariaDB、Oracle、DB2、H2、HSQL、SQLite、Postgre、SQLServer2005、SQLServer 等多种数据库
6. **支持主键自动生成**：支持多达 4 种主键策略（内含分布式唯一 ID 生成器 - Sequence），可自由配置，完美解决主键问题
7. **支持 XML 热加载**：Mapper 对应的 XML 支持热加载，对于简单的 CRUD 操作，甚至可以无 XML 启动
8. **支持 ActiveRecord 模式**：支持 ActiveRecord 形式调用，实体类只需继承 Model 类即可进行强大的 CRUD 操作
9. **支持自定义全局通用操作**：支持全局通用方法注入（ Write once, use anywhere ）
10. **支持关键词自动转义**：支持数据库关键词（order、key......）自动转义，还可自定义关键词
11. **内置代码生成器**：采用代码或者 Maven 插件可快速生成 Mapper 、 Model 、 Service 、 Controller 层代码，支持模板引擎，更有超多自定义配置等您来使用
12. **内置分页插件**：基于 MyBatis 物理分页，开发者无需关心具体操作，配置好插件之后，写分页等同于普通 List 查询
13. **内置性能分析插件**：可输出 Sql 语句以及其执行时间，建议开发测试时启用该功能，能快速揪出慢查询
14. **内置全局拦截插件**：提供全表 delete 、 update 操作智能分析阻断，也可自定义拦截规则，预防误操作
15. **内置 Sql 注入剥离器**：支持 Sql 注入剥离，有效预防 Sql 注入攻击

# 二、前期准备工作
## 2.1 创建并初始化数据库
### 创建数据库
mybatis_plus
### 创建User表

```sql
DROP TABLE IF EXISTS user;

CREATE TABLE user
(
	id BIGINT(20) NOT NULL COMMENT '主键ID',
	name VARCHAR(30) NULL DEFAULT NULL COMMENT '姓名',
	age INT(11) NULL DEFAULT NULL COMMENT '年龄',
	email VARCHAR(50) NULL DEFAULT NULL COMMENT '邮箱',
	PRIMARY KEY (id)
);
```
### 插入一些初始数据

```sql
DELETE FROM user;

INSERT INTO user (id, name, age, email) VALUES
(1, 'Jone', 18, 'test1@baomidou.com'),
(2, 'Jack', 20, 'test2@baomidou.com'),
(3, 'Tom', 28, 'test3@baomidou.com'),
(4, 'Sandy', 21, 'test4@baomidou.com'),
(5, 'Billie', 24, 'test5@baomidou.com');
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200920212115334.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)
## 2.2 初始化工程、添加依赖、数据库配置
### 使用 Spring Initializr 快速初始化一个 Spring Boot 工程
### 添加依赖
* spring-boot-starter、spring-boot-starter-test
* 添加：mybatis-plus-boot-starter、MySQL、lombok、
* 在项目中使用Lombok可以减少很多重复代码的书写。比如说getter/setter/toString等方法的编写。Lombok具体使用需要安装IDEA的插件可以参考[安装插件](https://blog.csdn.net/weixin_44870909/article/details/108041457)。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.2.1.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.garden</groupId>
    <artifactId>mabatisplus</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>mabatisplus</name>
    <description>Demo project for Spring Boot</description>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.0.5</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>

```

> 注意：引入 MyBatis-Plus 之后请不要再次引入 MyBatis 以及 MyBatis-Spring，以避免因版本差异导致的问题。
### 数据库配置
* 数据库配置的注意事项可以参考我的博客：[数据库配置注意事项](https://blog.csdn.net/weixin_44870909/article/details/108684470)
* **mybatis日志配置后 可以查看sql输出日志**
```properties
# mysql数据库连接
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/mp?serverTimezone=GMT%2B8
spring.datasource.username=root
spring.datasource.password=123456

# mybatis日志
mybatis-plus.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
```
# 三、编写代码
## 3.1 主启动类
在 Spring Boot 启动类中**添加 @MapperScan 注解，扫描 Mapper 文件夹**
**注意**：扫描的包名根据实际情况修改
```java
@SpringBootApplication
@MapperScan("com.garden.mybatisplus.mapper")
public class MybatisPlusApplication {
	......
}
```
## 3.2 实体类
创建包 entity 编写实体类 User.java（**此处使用了 Lombok 简化代码：添加了@Data注解**）
```java
@Data
public class User {
    private Long id;
    private String name;
    private Integer age;
    private String email;
}
```
### 3.3 mapper
创建包 mapper 编写Mapper 接口： UserMapper.java
**注意继承了BaseMapper<User>，这是MybatisPlus的写法**
```java
@Repository
public interface UserMapper extends BaseMapper<User> {
}
```
## 3.4 测试类
**添加测试类，进行功能测试**

* 查询user表所有数据
```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class MybatisPlusApplicationTests {

    @Autowired
    private UserMapper userMapper;

    @Test
    public void testSelectList() {
        System.out.println(("----- selectAll method test ------"));
        //UserMapper 中的 selectList() 方法的参数为 MP 内置的条件封装器 Wrapper
        //所以不填写就是无任何条件
        List<User> users = userMapper.selectList(null);
        users.forEach(System.out::println);
    }
}
```
* 之后都是代码都是在测试类进行功能测试

# 四、MyBatisPlus的CRUD 接口
## 4.1 添加操作
```java
    @Test
    public void addUser() {
        User user = new User();
        user.setName("岳不群1");
        user.setAge(70);
        user.setEmail("lucy@qq.com");
        int insert = userMapper.insert(user);
        System.out.println("insert:"+insert);
    }
```
## 4.2 删除操作：物理删除
* **通过id删除单条数据**
```java
    @Test
    public void testDeleteById(){
        int result = userMapper.deleteById(1231125349744828417L);
        System.out.println(result);
    }
```
* **利用id的Arrays批量删除**

```java
@Test
    public void testDeleteBatchIds() {
        int result = userMapper.deleteBatchIds(Arrays.asList(1,2));
        System.out.println(result);
    }
```

## 4.2 修改操作

```java
@Test
    public void updateUser() {
        User user = new User();
        user.setId(1231103936770154497L);
        user.setAge(120);
        int row = userMapper.updateById(user);
        System.out.println(row);
    }
```
## 4.3 查询操作
* **查询user表所有数据**
```java
    //查询user表所有数据
    @Test
    public void findAll() {
        List<User> users = userMapper.selectList(null);
        System.out.println(users);
    }
```
* **多个id批量查询**

```java
//多个id批量查询
    @Test
    public void testSelectDemo1() {
        List<User> users = userMapper.selectBatchIds(Arrays.asList(1L, 2L, 3L));
        System.out.println(users);
    }
```
* **利用map来按条件查询**

```java
@Test
    public void testSelectByMap(){
        HashMap<String, Object> map = new HashMap<>();
        map.put("name", "Jone");
        map.put("age", 18);
        List<User> users = userMapper.selectByMap(map);
        users.forEach(System.out::println);
    }
```
* **分页查询**
```java
//分页查询
    @Test
    public void testPage() {
        //1 创建page对象
        //传入两个参数：当前页 和 每页显示记录数
        Page<User> page = new Page<>(1,3);
        //调用mp分页查询的方法
        //调用mp分页查询过程中，底层封装
        //把分页所有数据封装到page对象里面
        userMapper.selectPage(page,null);

        //通过page对象获取分页数据
        System.out.println(page.getCurrent());//当前页
        System.out.println(page.getRecords());//每页数据list集合
        System.out.println(page.getSize());//每页显示记录数
        System.out.println(page.getTotal()); //总记录数
        System.out.println(page.getPages()); //总页数

        System.out.println(page.hasNext()); //下一页
        System.out.println(page.hasPrevious()); //上一页

    }
```

> 分页查询需要在配置类注册分页插件

```java
@Configuration
public class MpConfig {
    @Bean
    public PaginationInterceptor paginationInterceptor() {
        return new PaginationInterceptor();
    }
}
```


# 五、MybatisPlus的自动填充实例
##  5.1 自动填充使用场景
项目中经常会遇到一些数据，每次都使用相同的方式填充，例如记录的**创建时间，更新时间**等。我们可以使用MyBatis Plus的自动填充功能，完成这些字段的赋值工作：创建时自动填充该条数据的创建时间和更新时间，之后有对该条数据的更新操作时重新自动修改更新时间。
## 5.2 自动填充功能步骤
**1. 数据库表中添加自动填充字段**
在User表中添加datetime类型的新的字段 create_time、update_time
**2. 实体上添加注解@TableField**
@TableField注解中，
**fill = FieldFill.INSERT**：表示数据插入时该字段会自动创建
**fill = FieldFill.INSERT_UPDATE**：表示数据插入时该字段会自动创建，数据更新时该字段也会自动更新
* **字段的创建方式和更新方式需要在配置类中自定义**
```java
@Data
public class User {
    private Long id;

    private String name;
    private Integer age;
    private String email;

    //create_time
    @TableField(fill = FieldFill.INSERT)
    private Date createTime;

    //update_time
    @TableField(fill = FieldFill.INSERT_UPDATE)
    private Date updateTime;
}
```
**3. 实现元对象处理器接口（自定义自动填充方式）**
```java
@Component
public class MyMetaObjectHandler implements MetaObjectHandler {

    private static final Logger LOGGER = LoggerFactory.getLogger(MyMetaObjectHandler.class);

    @Override
    public void insertFill(MetaObject metaObject) {
        this.setFieldValByName("createTime", new Date(), metaObject);
        this.setFieldValByName("updateTime", new Date(), metaObject);
    }

    @Override
    public void updateFill(MetaObject metaObject) {
        this.setFieldValByName("updateTime", new Date(), metaObject);
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200920220037396.png#pic_center)
# 六、乐观锁
## 6.1 主要适用场景
当要更新一条记录的时候，希望这条记录没有被别人更新，也就是说实现线程安全的数据更新
## 6.2 乐观锁工作流程
* **乐观锁实现方式**
1. 取出记录时，获取当前version
2. 更新时，带上这个version
3. 执行更新时， set version = newVersion where version = oldVersion
4. 如果version不对，就更新失败
## 6.3 实现步骤
**1. 数据库中添加version字段**

```sql
ALTER TABLE `user` ADD COLUMN `version` INT
```
**2. 实体类添加version字段**

```java
@Data
public class User {

    //@TableId(type = IdType.ID_WORKER) //mp自带策略，生成19位值，数字类型使用这种策略，比如long
    //@TableId(type = IdType.ID_WORKER_STR) //mp自带策略，生成19位值，字符串类型使用这种策略
    private Long id;

    private String name;
    private Integer age;
    private String email;

    //create_time
    @TableField(fill = FieldFill.INSERT)
    private Date createTime;

    //update_time
    @TableField(fill = FieldFill.INSERT_UPDATE)
    private Date updateTime;

    @Version
    @TableField(fill = FieldFill.INSERT)
    private Integer version;//版本号
}
```
**3. 元对象处理器接口添加version的insert默认值**
* **this.setFieldValByName("version",1,metaObject);**
```java
@Component
public class MyMetaObjectHandler implements MetaObjectHandler {

    //使用mp实现添加操作，这个方法执行
    @Override
    public void insertFill(MetaObject metaObject) {
        this.setFieldValByName("createTime",new Date(),metaObject);
        this.setFieldValByName("updateTime",new Date(),metaObject);

        this.setFieldValByName("version",1,metaObject);
    }

    //使用mp实现修改操作，这个方法执行
    @Override
    public void updateFill(MetaObject metaObject) {
        this.setFieldValByName("updateTime",new Date(),metaObject);
    }
}
```

> 特别说明:
> 支持的数据类型只有 int,Integer,long,Long,Date,Timestamp,LocalDateTime
> 整数类型下 newVersion = oldVersion + 1
> newVersion 会回写到 entity 中
> 仅支持 updateById(id) 与 update(entity, wrapper) 方法
> 在 update(entity, wrapper) 方法下, wrapper 不能复用!!!

**4. 在 MybatisPlusConfig 中注册 乐观锁插件**

```java
@Configuration
@MapperScan("com.atguigu.mpdemo1010.mapper")
public class MpConfig {
    //乐观锁插件
    @Bean
    public OptimisticLockerInterceptor optimisticLockerInterceptor() {
        return new OptimisticLockerInterceptor();
    }
}
```
**5. 测试**
* **测试乐观锁可以修改成功**

```java
@Test
public void testOptimisticLocker() {

    //查询
    User user = userMapper.selectById(1L);
    //修改数据
    user.setName("Helen Yao");
    user.setEmail("helen@qq.com");
    //执行更新
    userMapper.updateById(user);
}
```

* * **测试乐观锁修改失败**

```java
@Test
public void testOptimisticLockerFail() {

    //查询
    User user = userMapper.selectById(1L);
    //修改数据
    user.setName("Helen Yao1");
    user.setEmail("helen@qq.com1");

    //模拟取出数据后，数据库中version实际数据比取出的值大，即已被其它线程修改并更新了version
    user.setVersion(user.getVersion() - 1);

    //执行更新
    userMapper.updateById(user);
}
```
# 七、逻辑删除
## 7.1 什么是逻辑删除
* **物理删除**：真实删除，将对应数据从数据库中删除，**之后查询不到此条被删除数据**
* **逻辑删除**：**假删除**，将对应数据中代表是否被删除字段状态修改为“被删除状态”，**之后在数据库中仍旧能看到此条数据记录**

## 7.2 逻辑删除实现步骤
**1. 数据库中添加 deleted字段**

```sql
ALTER TABLE `user` ADD COLUMN `deleted` boolean
```

**2. 实体类添加deleted 字段**
加上 @TableLogic 注解：表示逻辑删除的标记字段

```java
@Data
public class User {

    //@TableId(type = IdType.ID_WORKER) //mp自带策略，生成19位值，数字类型使用这种策略，比如long
    //@TableId(type = IdType.ID_WORKER_STR) //mp自带策略，生成19位值，字符串类型使用这种策略
    private Long id;

    private String name;
    private Integer age;
    private String email;

    //create_time
    @TableField(fill = FieldFill.INSERT)
    private Date createTime;

    //update_time
    @TableField(fill = FieldFill.INSERT_UPDATE)
    private Date updateTime;

    @Version
    @TableField(fill = FieldFill.INSERT)
    private Integer version;//版本号

    @TableLogic
    @TableField(fill = FieldFill.INSERT)
    private Integer deleted;
}
```

**3. 元对象处理器接口添加deleted的insert默认值**

```java
@Override
public void insertFill(MetaObject metaObject) {
    ......
    this.setFieldValByName("deleted", 0, metaObject);
}
```

**4. application.properties 加入配置**

```java
mybatis-plus.global-config.db-config.logic-delete-value=1
mybatis-plus.global-config.db-config.logic-not-delete-value=0
```

**5. 在 MybatisPlusConfig 中注册 Bean**

```java
@Bean
public ISqlInjector sqlInjector() {
    return new LogicSqlInjector();
}
```

**6. 测试逻辑删除**

* 测试后发现，数据并没有被删除，deleted字段的值由0变成了1
* 测试后分析打印的sql语句，是一条update
* 注意：被删除数据的deleted 字段的值必须是 0，才能被选取出来执行逻辑删除的操作

```java
@Test
public void testLogicDelete() {

    int result = userMapper.deleteById(1L);
    System.out.println(result);
}
```

**7. 测试逻辑删除后的查询**
MyBatis Plus中查询操作也会自动添加逻辑删除字段的判断

```java
@Test
public void testLogicDeleteSelect() {
    User user = new User();
    List<User> users = userMapper.selectList(null);
    users.forEach(System.out::println);
}
```
# 八、性能分析
## 8.1 功能描述
性能分析拦截器，用于输出每条 SQL 语句及其执行时间
SQL 性能执行分析,开发环境使用，超过指定时间，停止运行。有助于发现问题

## 8.2 配置插件
1. **参数说明**
	参数：maxTime： SQL 执行最大时长，超过自动停止运行，有助于发现问题。
	参数：format： SQL是否格式化，默认false。
2. **在 MybatisPlusConfig 中配置**

```java
/**
 * SQL 执行性能分析插件
 * 开发环境使用，线上不推荐。 maxTime 指的是 sql 最大执行时长
 */
@Bean
@Profile({"dev","test"})// 设置 dev test 环境开启
public PerformanceInterceptor performanceInterceptor() {
    PerformanceInterceptor performanceInterceptor = new PerformanceInterceptor();
    performanceInterceptor.setMaxTime(100);//ms，超过此处设置的ms则sql不执行
    performanceInterceptor.setFormat(true);
    return performanceInterceptor;
}
```
3. Spring Boot 中设置dev环境
* 可以针对各环境新建不同的配置文件application-dev.properties、application-test.properties、application-prod.properties
也可以自定义环境名称：如test1、test2
```java
#环境设置：dev、test、prod
spring.profiles.active=dev
```
## 8.3 测试
**1. 常规测试**

```java
/**
 * 测试 性能分析插件
 */
@Test
public void testPerformance() {
    User user = new User();
    user.setName("我是Helen");
    user.setEmail("helen@sina.com");
    user.setAge(18);
    userMapper.insert(user);
}
```
**输出**：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200920223006630.png#pic_center)
**2. 将maxTime 改小之后再次进行测试**

```java
performanceInterceptor.setMaxTime(5);//ms，超过此处设置的ms不执行
```
**输出：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200920223057720.png#pic_center)
