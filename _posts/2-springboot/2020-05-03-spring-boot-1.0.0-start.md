---
layout: post
title: SpringBoot启动过程(一)-SpringApplication对象创建
description: ""
modified: 2014-12-24
tags: [SpringBoot - 源码系列]

---

**万物皆有起源**

SpringBoot是一个优雅而复杂的框架，从此刻开始，我逐行分析其启动过程。

#### 一、SpringBoot启动源码-入口代码
```
//入口
@SpringBootApplication
public class MallWebApplication {
    public static void main(String[] args) {
        SpringApplication.run(MallWebApplication.class,args);
    }
}
```
以上便是SpringBoot的启动入口代码，简单的不得了，当`run`方法成功返回时，Spring也就启动成功了。

那到底怎么启动的呢？废话不多说，开始源码之旅。


**该部分涉及到的知识点：**

- Java注解的用法与原理
- Class对象的原理 

#### 二、 SpringBoot启动源码-创建SpringApplication对象
跟着`run`方法的源码进去不多时，即遇到了第一道关卡，代码中New了一个SpringApplication对象，现在来分析一下这个类是什么和创建这个类的时候做了什么。

```
public SpringApplication(ResourceLoader resourceLoader, Class<?>... primarySources) {
		this.resourceLoader = resourceLoader;
		Assert.notNull(primarySources, "PrimarySources must not be null");
		this.primarySources = new LinkedHashSet<>(Arrays.asList(primarySources));
		this.webApplicationType = WebApplicationType.deduceFromClasspath();
		setInitializers((Collection) getSpringFactoriesInstances(
				ApplicationContextInitializer.class));
		setListeners((Collection) getSpringFactoriesInstances(ApplicationListener.class));
		this.mainApplicationClass = deduceMainApplicationClass();
	}
```



**SpringApplication类**
  ```
  public class SpringApplication {
  }
  ```
  该类没有继承任何父类或接口，真是个单纯的类，在Spring里的很少见的了，所以该类是一个单纯的启动工具类，他的调用`run`方法后，返回的是`ConfigurableApplicationContext`这个才是真正的容器上下文。

**SpringApplication构造函数里构造了什么？**

跟着构造函数里的代码，我们逐行来：

**第一行：**`this.resourceLoader = resourceLoader;`
    
   resourceLoader的数据类型是：`ResourceLoader`,这个涉及到了Spring的资源加载机制：`Resource`，里面运用了`策略模式`，实现了以统一的方式对Spring可能运用到的资源进行访问。所以`resourceLoader`变量是一个资源的统一加载器，此时启动的时候赋的是`null`值，没有意义。

**第三行：**`this.primarySources = new LinkedHashSet<>(Arrays.asList(primarySources))`;
   
   参数`primarySources`是我们开发者自建的启动类的Class对象，这里就只是单纯的把该对象存储进`SpringApplication`对象的成员字段`primarySources`中，以备后用。

**第四行：** `this.webApplicationType = WebApplicationType.deduceFromClasspath();`

深入去看该方法的实现：
```
static WebApplicationType deduceFromClasspath() {
		if (ClassUtils.isPresent(WEBFLUX_INDICATOR_CLASS, null)
				&& !ClassUtils.isPresent(WEBMVC_INDICATOR_CLASS, null)
				&& !ClassUtils.isPresent(JERSEY_INDICATOR_CLASS, null)) {
			return WebApplicationType.REACTIVE;
		}
		for (String className : SERVLET_INDICATOR_CLASSES) {
			if (!ClassUtils.isPresent(className, null)) {
				return WebApplicationType.NONE;
			}
		}
		return WebApplicationType.SERVLET;
	}

```
首先，我们理解一下这个方法是要干嘛；

`deduce`的中文是：`推断`;`deduceFromClasspath`的意思就是从Classpath中推断是某个事情的意思，然后他的返回值类型：`WebApplicationType`,所以该方法就是通过Classpath中推断出应用的类型。那么应用有多少种类型呢? 看代码我发现有三种：

- `WebApplicationType.NONE`
  
  该类型的应用不是Web应用，所以也不会启动内嵌的Web服务器
- `WebApplicationType.SERVLET`
  
  该类型的应用是servletWeb应用程序，运行时会启动一个内嵌的servlet服务器
- `WebApplicationType.REACTIVE`

    该类型的应用是响应式应用，运行时会启动一个内嵌的响应式服务器


结论很清晰，这个方法就是用来确定该应用的类型，`SERVLET`对应我们最经常用的`SpringMVC`，而`REACTIVE`则对应的是最新的`Spring-Webflux`

而目前我们绝大部分还是使用SpringMVC，所以推断的结果基本还是`SERVLET`,所以最终启动的就是`Tomcat`服务器

**第五行：**`setInitializers((Collection) getSpringFactoriesInstances(ApplicationContextInitializer.class));`

这行代码需要静下心来揣摩和理解，它涉及到了Spring的蛮多方面，由于述说起来篇幅较长，另外一篇文章述说：

传送门：[Spring-初始化器(Initializers)]({{site.url}}/spring-2.0.1-Initializers/)

**第六行：**`setListeners((Collection) getSpringFactoriesInstances(ApplicationListener.class));`

这行代码需要静下心来揣摩和理解，它涉及到了Spring的蛮多方面，由于述说起来篇幅较长，另外一篇文章述说：

传送门：[Spring-事件监听器器(Listener)]({{site.url}}/spring-2.0.1-Listener)

**第七行：**`this.mainApplicationClass = deduceMainApplicationClass();`
推断出含有main函数的类

至此，创建`SpringApplication`对象的代码分析完毕了

**总结：**
- 设置了默认初始化器
- 设置了默认监听器
















