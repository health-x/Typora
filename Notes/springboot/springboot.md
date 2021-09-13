## 一、配置文件加载优先级

### 1. 文件夹级别：

- file:../config/：当前项目下的config目录下（在项目根目录里建个config目录）
- file:../：当前项目的根目录（若有父项目，则是父项目的根目录）
- classpath:/config/：classpath的/config目录
- classpath:/：classpath的根目录

### 2. 文件级别：

- 文件名：bootstrap > application
- 后缀名：properties > yml > yaml



## 二、读取配置文件内容

```java
@Value("${person.name}")	//方法1：注解读取

@Autowired					//方法2：对象读取
private Environment env;
env.getProperty("person.name")
    
@Data						//方法3：ConfigurationProperties读取						
@Component
@ConfigurationProperties(prefix = "person")
public class Person {
    private String name;
    private Integer age;
}
@Autowired
private Person person;
```



## 三、Profile

### 1) 介绍：

profiles是用来在不同环境下，动态切换不同配置功能的

### 2) profile配置方式：

1. 多配置文件方式
   - application-dev.yml 开发环境
   - application-test.yml 测试环境
   - application-pro.yml 生产环境
2. yml多文档方式
   - 在同一个yml文件中使用 --- 分隔不同的配置

### 3) profile激活方式

- 配置文件：在配置文件中配置：spring.profiles.active=dev
- 虚拟机参数：在VM options指定：-Dspring.profiles.active=dev
- 命令行参数：java -jar xxx.jar  --spring.profiles.active=dev

## 四、获取其他module自动注入的bean

```java
方法一：
@ComponentScan("类路径")
    
方法二：
@Import(类名.class)
    
方法三：
对@Import注解进行封装
步骤：
1.在其他module中编写自定义注解，覆写@Import注解    
@Target({ElementType.TYPE})	//@Import注解中的注解
@Retention(RetentionPolicy.RUNTIME)	//@Import注解中的注解
@Documented	//@Import注解中的注解
@Import(类名.class)	//类名，下方用到了
public @interface EnableUser{}
2.使用的时候直接使用@EnableUser即可获取到“类名”这个对象
```





































