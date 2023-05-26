[TOC]

#### Spring简介

##### Spring的优势

- `AOP`编程的支持
- 申明式事务的支持
- 方便程序的测试(SpringTest)
- 生态丰富，方便集成其他框架



##### 三个核心概念

###### IOC

- 控制反转
- 由容器来创建对象
- 采用了控制反转模式
- IOC的实现方式
  - 纯XML配置
  - 纯注解模式
  - XML+注解

###### AOP

- 面向切面
- 将重复的代码抽取出来
- 采用了动态代理模式

###### DI

- 依赖注入(和`IOC`可以看作是一个东西)
- 在spring创建Bean时,动态的将依赖的对象注入Bean对象中



##### 七大核心模块

###### Spring Core

- `Core`提供最核心的部分
- 包括依赖注入特性和IOC
- 基础概念BeanFactory,提供最Factory模式的实现

###### Spring AOP

- 提供符合AOP Alliance规范的面向切面的编程实现
- 可以自由定义拦截器和切点
- 减少代码的功能耦合

###### Spring Context

- 为Spring框架提供上下文信息
- 包括JNDI,EJB,电子邮件,国际化,校验,调度功能等

###### Spring DAO

- 提供了JDBC的抽象层
- 提供声明式的事务管理方法
- 适用与所有的POJO(Plain old java objects)

###### Spring ORM

- 提供了常用的对象/关系映射API的集成层
- 包括`JPA`,`JDO`,`Hibernat`,`iBatis`

###### Spring Web

- 提供了针对web开发的集成特性
- 包括文件上传,利用Servlet Listeners进行IOC容器初始化针对web的ApplicationContext

###### Spring Web MVC

- 提供了MVC(Model-View-Controller)的实现



##### 核心类

###### DispatcherServlet

- 主要职责是负责调度工作,本身用于调度流程
- 通过**HandlerMapping**,将请求映射到处理器(返回一个HandlerExecutionChain，它包括一个处理器、多个HandlerInterceptor拦截器)
- 通过**handlerAdapter**支持多种类型的处理器
- 通过**ViewResolver**解析逻辑视图名到具体视图实现
- 如果执行过程中遇到异常将交给**HandlerExceptionResolver**来解析



##### 常用注解和功能

###### 定义Bean

- @Component,@Service,@Controller,@Repository
- 其实是同一个东西,为了区分不同的应用场景

###### 注入Bean

- @Autowired:根据类型注入Bean
- @Qualifier:可以结合@Autowired注解,根据name确定唯一的Bean
- @Resource:根据name注入Bean

###### 配置相关

- @ComponentScan:定义扫描路径,从中寻找需要装配的Bean
- @Configuration:表示当前类是一个配置类
- @Import:引入其他配置类
- @PropertySource:引入外部属性配置文件
- @Value:对变量赋值,可以直接赋值,也可以通过`${}`读取配置文件的信息赋值
- @Bean:将方法返回的对象加入`Spring IOC`容器

###### AOP相关

- 需要定义为Bean,使用@Component注解(或等效的注解)
- @Aspect:表明这是一个切面
- @Pointcut:定义切点表达式
  - "execution(* com.demo.service.DemoService.Text(..))"
- @Before:在目标方法调用前执行
- @After:在目标方法返回或异常后执行
- @AfterReturn:在目标方法返回后执行
- @AfterThrowing:在目标方法异常后执行
- @Around:封装目标方法

###### 事务相关

- @Transactional:添加事务和设置一些基本属性
  - 传播行为:propagation
  - 隔离级别:isolation
  - 回滚规则:rollbackFor/rollbackForClassName
  - 事务超时:timeout
  - 是否只读:readOnly
