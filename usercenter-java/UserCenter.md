# 用户中心

完整了解做项目的思路,接触一些企业级的开发技术 (尽量少写代码)目标: 之后可以轻松做出管理项目

# 企业做项目流程

需求分析 => 设计(概要设计,详细设计) => 技术选型 => 初始化/引入需要的技术 =>  写 Demo => 写代码(实现业务逻辑) => 测试(单元测试)  => 代码提交/代码评审 => 部署 =>发布

# 需求分析

1.    **登录/注册**
2.   **用户管理(仅管理员)** **对用户的查询或修改**
3.   用户校验 (**仅星球用户**)

# 技术选型

前端: 三件套 + React + 组件库 Ant Design + Umi + Ant Design Pro (现成的管理系统)

后端:  

-   Java 
-   spring (依赖注入框架,帮助你管理java对象,集成一些其他的内容)
-   springmvc (web框架,提供接口访问,restful接口等能力)
-   mybatis (java操作数据库的框架,持久层框架,对jdbc的封装)
-   mybatis-plus (对mybatis的增强,不用写sql也能实现增删改查)
-   springboot (快速启动/快速集成项目.不需要自己管理spring配置,不用自己整合各种框架)
-   junit  单元测试库
-   mysql

部署: 服务器/容器 (平台)

# 计划

1.   初始化项目
     1.   初始化前端
          1.   初始化项目
          2.   引入一些组件
          3.   项目瘦身
     2.   后端初始化
          1.   准备环境(mysql之类的)
          2.   引入框架
2.    **登录/注册**
     1.   后端
          1.   规整项目目录
          2.   实现基本的数据库操作(操作user表)
               1.   模型 user对象 => 和数据库的字段关联, 自动生成
          3.   写登录逻辑
     2.   前端
3.   **用户管理(仅管理员)**
     1.   前端
     2.   后端

# 数据库设计

什么是设计数据库表?

有哪些表? 表中有哪些字段? 字段的类型? 数据库字段添加索引? 

表与表之间的关联~



用户表:

id (主键) varchar

username: 昵称 varchar

userAccount 登录账号

avatarUrl 头像 varchar

gender 性别 tinyint

password 密码 varchar

phone 电话 varchar

email 邮箱 varchar

isValid 是否有效(被封号之类的)  tinyint

createTime 创建时间 (数据插入时间)  Datetime

updateTime 更新时间 (数据更新时间) Datetime

isDelete 是否删除 0 1 (逻辑删除) tinyint

### 自动生成器的使用

MyBatisX 插件 ,自动根据数据库生成domain实体对象,mapper(操作数据库的对象),mapper.xml(定义了mapper对象和数据库的关联,可以在里面自己写sql),service(包含了常用的增删改查) serviceImpl(service实现类)

### 注册逻辑

1.   用户在前端输入账户和密码,以及校验码(todo)
2.   校验用户的账户,密码,校验密码是否符合要求
     1.   非空
     2.   账户不小于4位 
     3.   密码不小于8位
     4.   账户不能重复
     5.   账户不包含特殊字符
     6.   密码和校验密码相同
3.   对密码进行加密(密码一定不能明文存储到数据库中)
4.   向数据库插入用户数据

### 登录逻辑

接收参数: 用户账号,密码

请求类型: POST

请求体: JSON格式的数据

[^请求参数很长时不建议使用get请求]:

返回值: 用户信息(**脱敏**)

#### 逻辑

1.   校验用户账户和密码是否合法
     1.   非空
     2.   账户长度不小于4位
     3.   密码不小于8位
     4.   账户不包含特殊字符
2.   校验密码是否输入正确,要和数据库中的密文密码去对比
3.   要记录用户的登录态(session),存储到服务器(用后端Springboot框架封装的服务器tomcat去记录)