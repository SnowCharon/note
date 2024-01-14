# Spring

## Bean

### 注入方法：

1. 属性注入
2. 构造器注入
3. setter注入
## Exception

 当测试类运行出现该报错：

```java
java.lang.NoSuchMethodError: org.springframework.test.context.TestContext.computeAttribute
```

**即表示pop.xml中有jar版本不同，出现冲突**
## 注解

### @Bean

Spring的@Bean注解用于告诉方法，产生一个Bean对象，然后这个Bean对象交给Spring管理。产生这个Bean对象的方法Spring只会调用一次，随后这个Spring将会将这个Bean对象放在自己的IOC容器中。

SpringIOC 容器管理一个或者多个bean，这些bean都需要在@Configuration注解下进行创建，在一个方法上使用@Bean注解就表明这个方法需要交给Spring进行管理。





### @Autowired

**功能：它可以对类成员变量、方法及构造函数进行标注，完成自动装配的工作**

原理：

​	通过@Autowired自动装配方式，从IoC容器中去查找到，并返回给该属性——需要开启Bean的扫描

**applicationContext.xml配置**

```java
<context:component-scan base-package="com.proc.bean" />
```

 **注意事项：**

　在使用@Autowired时，首先在容器中查询对应类型的bean

　如果查询结果刚好为一个，就将该bean装配给@Autowired指定的数据

　如果查询的结果不止一个，那么@Autowired会根据名称来查找。

　如果查询的结果为空，那么会抛出异常。解决方法时，使用required=false

缺点：

默认情况下，`@Autowired` 按类型装配 **Spring Bean**。如果容器中有多个相同类型的 **bean**，则框架将抛出 `NoUniqueBeanDefinitionException`， 以提示有多个满足条件的 **bean** 进行自动装配，程序无法正确做出判断使用哪一个——可以使用`@Qualifier`注解解决

```java
 @Component("fooFormatter")
    public class FooFormatter implements Formatter {
        public String format() {
            return "foo";
        }
    }

    @Component("barFormatter")
    public class BarFormatter implements Formatter {
        public String format() {
            return "bar";
        }
    }

    @Component
    public class FooService {
        @Autowired
        private Formatter formatter;
        
        //todo 
    }
```

### @Qualifier

解决多个Bean让`@AutoWired`无法选择的问题：

`@Component`  或者`@Bean`注解中声明的value属性以确定名称,也可以在 `Formatter` 实现类上使用 `@Qualifier` 注释，而不是在 `@Component` 或者 `@Bean` 中指定名称，也能达到相同的效果.

```java
@Component("fooFormatter")
public class FooFormatter implements Formatter {
    public String format() {
        return "foo";
    }
}

@Component("barFormatter")
public class BarFormatter implements Formatter {
    public String format() {
        return "bar";
    }
}

@Component
public class FooService {
    @Autowired
    private Formatter formatter;

    //todo 
}  
@Component
    public class FooService {
        @Autowired
        @Qualifier("fooFormatter")
        private Formatter formatter;
        
        //todo 
    }
```



### @Primary

该注解可以提升某个Bean在同类型Bean中的优先级，如果 **@Primary** 与 **@Qualifier**同时存在优先生效**@Qualifier**，因为它是根据名称指定，优先级更高，当然如果 **@Autowired** 注解的变量名称符合Bean的名称规范——即首单词小写，后面的单词首字母大写，那么Spring不会脑分裂，可以自动装配成功。



### @Transactional

声明式事务管理建立在AOP之上的。其本质是对方法前后进行拦截，然后在目标方法开始之前创建或者加入一个事务，在执行完目标方法之后根据执行情况提交或者回滚事务。

#### 使用方法

- 在启动类上添加@EnableTransactionManagement注解。
- 用于类上时，该类的所有 public 方法将都具有该类型的事务属性，同时，我们也可以在方法级别使用该标注来覆盖类级别的定义。
- 在项目中，@Transactional(rollbackFor=Exception.class)，如果类加了这个注解，那么这个类里面的方法抛出异常，就会回滚，数据库里面的数据也会回滚。
- 在@Transactional注解中如果不配置rollbackFor属性,那么事物只会在遇到RuntimeException的时候才会回滚,加上rollbackFor=Exception.class,可以让事物在遇到非运行时异常时也回滚。

#### 失效可能原因

1. @Transactional 应用在非 public 修饰的方法上
2. @Transactional 注解属性 rollbackFor 设置错误
3. 同一个类中方法调用，导致@Transactional失效
4. 同一个类中方法调用，导致@Transactional失效
5. 数据库引擎不支持事务
6. 数据库引擎不支持事务

#### 属性介绍

##### Value:指定事务的限定符值

##### propagation:事务传播类型

##### isolation：事务隔离级别

##### timeout：事务超时(以秒为单位)

##### readOnly：只读标志

##### rollbackFor：指定在触发哪些异常时回滚事务

