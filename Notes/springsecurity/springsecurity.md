# spring security

## 一、最简

1.引入依赖

```xml
<!--spring security-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

2.编写controller类

```java
@Controller
@RequestMapping("/user")
public class UserController {

    @GetMapping("/getUser")
    public String hello() {
        return "访问成功！";
    }
}
```

此时启动项目，访问 **localhost:8080/user/getUser** 时会自动跳转到 springsecurity 自带的登录界面，默认用户名为：user，密码会在控制台输出。

## 二、进阶：

1.引入依赖

```xml
<!--spring security-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

2.编写controller类，和前端页面

```java
@Controller
@RequestMapping("/user")
public class UserController {

    @Autowired
    private IUserService userService;

    @GetMapping("/show01")
    public String hello() {
        //因为在配置类中放行了该请求
        return "不用登录就可以访问";
    }

    @GetMapping("/query")
    @ResponseBody
    public List<User> query() {
        return userService.queryUserList();
    }

    @GetMapping("/login")
    public String login() {
        return "redirect:./login.html";
    }

    @PostMapping("/success")
    public String success() {
        return "redirect:../success.html";
    }

    @PostMapping("/error")
    public String error() {
        return "redirect:../error.html";
    }

}
```

login.html

```html
<body>
<h1>登陆</h1>
<form method="post" action="/user/login">	这里只能是post
    用户名：<input type="text" name="username123" placeholder="请输入用户名">	 这里的name值与配置类中的对应
    <br>
    密 码：<input type="password" name="password123" placeholder="请输入密码">
    <br>
    <input type="submit" value="登录">
</form>
</body>
```

3.编写spring security自带的UserDetailsService的实现类

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.authority.AuthorityUtils;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.stereotype.Service;

/**
 * spring security自带的UserDetailsService的实现类
 *
 * @author health
 * @since 2021/9/22
 */
@Service
public class UserDetailServiceImpl implements UserDetailsService {

    @Autowired
    private PasswordEncoder pwd;
    @Autowired
    private IUserService userService;

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        //1.查询数据库。判断该username是否存在，不存在则抛出UsernameNotFoundException异常
        com.health.security01.entity.User user = userService.queryUserByUsername(username);

        if (user==null) {
            throw new UsernameNotFoundException("用户不存在");
        }
        //2.把查询到的密码（加密的）进行解析，活直接把密码放入构造方法
        String password = pwd.encode(user.getPassword());
        return new User(username, password, AuthorityUtils.commaSeparatedStringToAuthorityList("admin,normal"));
    }
}
```

4.编写配置类

```java
import com.health.security01.handle.MyAuthenticationFailureHandler;
import com.health.security01.handle.MyAuthenticationSuccessHandler;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.http.HttpMethod;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.builders.WebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;

import java.util.Objects;

/**
 * security配置类
 *
 * @author health
 * @since 2021/9/22
 */
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    /**
     * 指定密码的加密方式，不然定义认证规则那里会报错
     */
    @Bean
    public PasswordEncoder getPw() {
        return new BCryptPasswordEncoder();
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        //设置拦截和放行规则（授权认证）
        http.authorizeRequests()
                //login.html不需要认证
                .antMatchers("/login.html").permitAll()
                //error.html不需要认证
                .antMatchers("/error.html").permitAll()
                //放行静态资源
                .antMatchers("/js/**", "/css/**", "/fonts/**", "/images/**", "/lib/**", "/user/show01").permitAll()
                //正则表达式
                .regexMatchers(HttpMethod.POST, "这里写表达式(表示post请求切符合正则的才会被放行)").permitAll()
                //重要：只有拥有admin权限的用户才能访问success02.html界面（hasAnyAuthority("admin","normal")可配置多个权限，逗号隔开）
                .antMatchers("/success02.html").hasAuthority("admin")
                //所有请求都必须被认证（除了上面放行的）,登陆之后才能访问
                .anyRequest().authenticated();

        //开启表单验证
        http.formLogin()
                .loginPage("/login.html")   //自定义登录界面
                .loginProcessingUrl("/user/login")  //当请求是/user/login（必须与表单请求对应）时，认为是登录，才会去执行UserDetailServiceImpl中的方法
                .successHandler(new MyAuthenticationSuccessHandler("http://www.baidu.com")) //登录成功的处理器,可用于前后端分离（不能与successForwardUrl共存）
                .failureHandler(new MyAuthenticationFailureHandler("/error.html")) //登录失败的处理器,可用于前后端分离（不能与failureForwardUrl共存）
                //.successForwardUrl("/user/success") //登陆成功跳转的页面(必须是post请求)
                //.failureForwardUrl("/user/error")   //登陆失败跳转的页面(必须是post请求)
                .usernameParameter("username123")   //这里要与login.html界面的对应name属性值对应，默认是username
                .passwordParameter("password123")  //默认时password
        ;
        //关闭csrf
        http.csrf().disable();
    }
}
```



## 三、知识

配置类：

```java
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {	//需要继承WebSecurityConfigurerAdapter类

    /**
     * 指定密码的加密方式，不然定义认证规则那里会报错
     */
    @Bean
    public PasswordEncoder getPw() {
        return new BCryptPasswordEncoder();
    }


    @Override
    protected void configure(HttpSecurity http) throws Exception {
        //设置拦截和放行规则（授权认证）
        http.authorizeRequests()
            	/*
            		放行配置
            	*/
                //放行具体页面(也可使用*，？，**等多匹配放行)，也可以放行某个或多个请求
                .antMatchers("/error.html"，"/hello/show01").permitAll()
                //放行静态资源
                .antMatchers("/js/**", "/css/**", "/fonts/**", "/images/**", "/lib/**").permitAll()
                //正则表达式
                .regexMatchers(HttpMethod.POST, "这里写表达式(表示post请求切符合正则的才会被放行)").permitAll()
            
            	/*
                    权限配置
            	*/
                //只有拥有admin权限的用户才能访问success02.html界面
                .antMatchers("/success02.html").hasAuthority("admin")
            	//拥有admin或normal权限的用户才能访问success02.html界面）
            	.antMatchers("/success02.html").hasAnyAuthority("admin","normal")
            
            	/*
            		角色配置
            	*/
            	.antMatchers("/success02.html").hasRole("abc")
                .antMatchers("/success02.html").hasAnyRole("abc","Abc")
            
            	/*
            		ip配置
            	*/
            	.antMatchers("/success02.html").hasIpAddress("127.0.0.1")
           
                //所有请求都必须被认证（除了上面放行的）,登陆之后才能访问
                .anyRequest().authenticated();

        
        //开启表单验证
        http.formLogin()
                .loginPage("/login.html")   //自定义登录界面（检测到未登录，会自动重定向到该页面）
                .loginProcessingUrl("/user/login")  //当请求是/user/login（必须与表单请求对应）时，认为是登录，才会去执行UserDetailServiceImpl中的方法
            	
            	/*
            		登陆成功与失败的处理（xxxxHandler不能与xxxxForwardUrl共存）
            		使用 xxxxhandler 需要自己手写AuthenticationXxxxHandler的实现类
            	*/
                .successHandler(new MyAuthenticationSuccessHandler("http://www.baidu.com")) //可用于前后端分离
            	.successForwardUrl("/user/success") //登陆成功跳转的页面(必须是post请求)
                .failureHandler(new MyAuthenticationFailureHandler("/error.html")) //可用于前后端分离
                .failureForwardUrl("/user/error")   //登陆失败跳转的页面(必须是post请求)
            
            	/*
            		前端想改form表单的name属性值的话，这里需要配置下，与其对应
            	*/
                .usernameParameter("username123")   //这里要与login.html界面的对应name属性值对应，默认是username
                .passwordParameter("password123")  //默认时password
        ;

        //关闭csrf(必须)
        http.csrf().disable();
    }

}
```























































