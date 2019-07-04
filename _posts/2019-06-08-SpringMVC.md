# SpringMVC
## dispatcherServlet
dispatcherServlet 拦截规则中的请求，发送到目标 controller 处理请求.
Spring 支持 xml 和注解两种配置方式。
ApplicationContext 对象,代表 Spring 控制翻转容器，
org.springframework.context.ApplicationContext 接口有多个实现:
ClassPathXmlApplicationContext 和 FileSystemXmlApplicationContext,两者都需要
至少有一个 beans 的 XML 文件，
```java
ApplicationContext ctx = new ClassPathXmlApplicationContext(new String[]{"config1.xml","config2.xml"});
Product product = ctx.getBean("product",Product.class); // id 为 product,且类型为 Product 的 bean 对象.
```
ApplicationContext ctx = 
