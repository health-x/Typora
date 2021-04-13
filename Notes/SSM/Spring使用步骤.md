# Spring 应用

### 一、入门案例：IOC

#### 1.导入jar包

4+1（4核心+1个依赖）

#### 2.目标类

```package com.health.ioc;
public interface UserService {
	public void addUser();
}
```

``` 
public class UserServiceImpl implements UserService{
	@Override
	public void addUser() {
		System.out.println("ioc_addUser");
	}
}
```



#### 3.配置文件

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- 配置service -->
    <bean id="UserServiceId" class="com.health.ioc.UserServiceImpl"></bean>

</beans>
```



#### 4.测试

```
package com.health.test;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import com.health.ioc.UserService;
import com.health.ioc.UserServiceImpl;

public class TestIoc {
	@Test
	public void Test01() {
		//之前开发
		UserService userService = new UserServiceImpl();
		userService.addUser();
	}
	
	@Test
	public void Test02() {
		//从容器中获得
		ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
		UserService userService =  (UserService)context.getBean("UserServiceId");
		userService.addUser();
	}
}
```



### 二、案例：DI

#### 1.目标类

* 创建BookService接口和实现类

* 创建BookDao接口和实现类
* 将dao和service配置到xml文件中

##### 1.1dao

```
public interface BookDao {
	public void addBook();
}
```

``` 
public class BookDaoImpl implements BookDao{
	@Override
	public void addBook() {
		// TODO Auto-generated method stub
		System.out.println("di add Books");
	}
}
```

##### 1.2service

``` 
public interface BookService {
	public void addBook();
}
```

```
public class BookServiceImpl implements BookService{
	//方式1：之前，接口＝实现类
	//private BookDao bookDao = new BookDaoImpl();
	
	
	//方式2：接口+setter
	private BookDao bookDao;
	public void setBookDao(BookDao bookDao) {
		this.bookDao = bookDao;
	}
	@Override
	public void addBook() {
		this.bookDao.addBook();
	}
}
```

#### 2.配置文件

``` 
<bean id="BookService" class="com.health.di.BookServiceImpl">
	<property name="bookDao" ref="BookDao"></property>
</bean>
<bean id="BookDao" class="com.health.di.BookDaoImpl"/>
```

#### 3.测试

```
public class TestDi {
	@Test
	public void test() {
		ApplicationContext ac = new ClassPathXmlApplicationContext("beans.xml");
		BookService bs = (BookService)ac.getBean("BookService");
		bs.addBook();
	}
}
```



### 三、装配Bean基于XML

#### 1.实例化方式

默认构造、静态工厂、实例工厂

1.2和1.3都用到的service类

```
public interface UserService {
	public void addUser();
}
```

```
public class UserServiceImpl implements UserService{
	@Override
	public void addUser() {
		System.out.println("factory_ioc_addUser");
	}
}
```

##### 1.1默认构造

<bean id="" class="">

##### 1.2静态工厂

常用于spring整合其他框架(工具)

###### 工厂

``` 
public class MyBeanFactory {
	/**
	 * 创建实例
	 * @return
	 */
	public static UserService createService() {
		return new UserServiceImpl();
	}
}
```

###### 配置

```
<bean id="userService" class="com.health.ZhuangPeiBean.static_factory.MyBeanFactory" factory-method="createService"/>
```

###### 测试

```
@Test
public void Test02() {
	ApplicationContext ac= new ClassPathXmlApplicationContext("beans.xml");
	UserService userService = ac.getBean("userService",UserService.class);
	userService.addUser();
}
```

##### 1.3实例工厂

特点：必须先有工厂的实例对象，通过实例对象创建对象。提供所有的方法都是非静态的。

###### 工厂

```
public class MyBeanFactory {
	/**
	 * 创建实例
	 * @return
	 */
	public UserService createService() {
		return new UserServiceImpl();
	}
}
```

###### 配置

```
<!-- factory -->
<bean id="myBeanFactory" class="com.health.ZhuangPeiBean.factory.MyBeanFactory"></bean>
<bean id="userService4" factory-bean="myBeanFactory" factory-method="createService"></bean>
```

###### 测试

```
@Test
public void Test02() {
	ApplicationContext ac= new ClassPathXmlApplicationContext("beans.xml");
	UserService userService = ac.getBean("userService4",UserService.class);
	userService.addUser();
}
```

#### 2.作用域

<bean id="" class="" scope="单例、多例、session······">

#### 3.生命周期

A a = new A();

a = B.before();	//将a的实例对象传递给后处理bean, 可以生成代理对象并返回。

a.init();

a = B.after();

a.addUser();

a.destroy();



（1）<bean id="" class="" init-method="" destroy-method="">

初始化方法和销毁方法(销毁方法需要将spring容器关闭才会执行，可调用ClasXmlPathApplicationContent的close()方法 或者 使用反射)

（2）实现BeanPostProcessor接口（初始化 前方法和后方法）



#### 4.属性依赖注入

##### 4.1 构造方法

##### 4.2 setter方法

##### 4.3 P命名空间

上面加：**xmlns:p="http://www.springframework.org/schema/p"（一般加在第三行）**

<bean id="" class="" p:属性=“xxx” p:属性=“xxx”>

##### 4.4 SpEL [了解]



#### 5.集合注入

#### 



# 四.AOP

### 一、术语

连接点：类中可以被增强的方法。

切入点：实际增强的方法

通知(增强)：实际增强的逻辑部分，总共有五种通知类型（前置/后置/环绕/异常/最终通知）

切面：把通知应用到切入点的过程