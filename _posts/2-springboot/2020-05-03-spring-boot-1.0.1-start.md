---
layout: post
title: SpringBoot启动过程(二)
description: ""
modified: 2014-12-24
tags: [SpringBoot - 源码系列]

---

**万物皆有起源**

SpringBoot是一个优雅而复杂的框架，从此刻开始，我逐行分析其启动过程。

接上文：[SpringBoot启动过程(一)-SpringApplication对象创建](/404.md)

创建完`SpringApplication`对象后，就开始调用`run`方法，这里是最复杂的了,一行代码都可能代表spring的一种机制。

```
public ConfigurableApplicationContext run(String... args) {
		StopWatch stopWatch = new StopWatch();
		stopWatch.start();
		ConfigurableApplicationContext context = null;
		Collection<SpringBootExceptionReporter> exceptionReporters = new ArrayList<>();
		configureHeadlessProperty();
		SpringApplicationRunListeners listeners = getRunListeners(args);
		listeners.starting();
		try {
			ApplicationArguments applicationArguments = new DefaultApplicationArguments(
					args);
			ConfigurableEnvironment environment = prepareEnvironment(listeners,
					applicationArguments);
			configureIgnoreBeanInfo(environment);
			Banner printedBanner = printBanner(environment);
			context = createApplicationContext();
			exceptionReporters = getSpringFactoriesInstances(
					SpringBootExceptionReporter.class,
					new Class[] { ConfigurableApplicationContext.class }, context);
			prepareContext(context, environment, listeners, applicationArguments,
					printedBanner);
			refreshContext(context);
			afterRefresh(context, applicationArguments);
			stopWatch.stop();
			if (this.logStartupInfo) {
				new StartupInfoLogger(this.mainApplicationClass)
						.logStarted(getApplicationLog(), stopWatch);
			}
			listeners.started(context);
			callRunners(context, applicationArguments);
		}
		catch (Throwable ex) {
			handleRunFailure(context, ex, exceptionReporters, listeners);
			throw new IllegalStateException(ex);
		}

		try {
			listeners.running(context);
		}
		catch (Throwable ex) {
			handleRunFailure(context, ex, exceptionReporters, null);
			throw new IllegalStateException(ex);
		}
		return context;
	}

```

复杂的事情要简单化，按照惯例，我们还是逐行分析

#### 第1~2行
```
StopWatch stopWatch = new StopWatch();
stopWatch.start();
```
这个是时间监听器，用于统计程序的执行时长的，不是主线，暂不分析。

#### 第3~4行
```
ConfigurableApplicationContext context = null;
Collection<SpringBootExceptionReporter> exceptionReporters = new ArrayList<>();
```
- 定义一个ConfigurableApplicationContext对象，请注意，笼统来说，spring的启动过程就是装配这个对象的过程。
- 初始化了一个`SpringBootExceptionReporter`集合，该集合类似`Listeners`,也是一个钩子，会被spring回调，由于这里是初始化，暂且不展开分析
  

#### 第5行
```
configureHeadlessProperty();

-------------
//点击进入该方法
private void configureHeadlessProperty() {
	System.setProperty(SYSTEM_PROPERTY_JAVA_AWT_HEADLESS, System.getProperty(SYSTEM_PROPERTY_JAVA_AWT_HEADLESS, Boolean.toString(this.headless)));
}
//备注：
private static final String SYSTEM_PROPERTY_JAVA_AWT_HEADLESS = "java.awt.headless";
```
我提炼了一下该方法如下：
```
System.setProperty("java.awt.headless", "true");
```
就是把系统中的`java.awt.headless`设置为`true`，有什么用？

---
`headless`其实是一种运行配置模式、一种标志；他指示该程序运行的环境(服务器)可能缺少显示设备、键盘、鼠标等设备，要求程序如果在需要这些设备时，通过计算机自行模拟。

现我们的springboot程序在生产环境，几乎都是部署在Linux服务器上的，所以都没有其他外设。


#### 第6~7行
```
SpringApplicationRunListeners listeners = getRunListeners(args);
listeners.starting();
```

这是spring的事件监听机制，我们一步步来刨析

首先，进入`getRunListeners(args)`方法
```
private SpringApplicationRunListeners getRunListeners(String[] args) {
		Class<?>[] types = new Class<?>[] { SpringApplication.class, String[].class };
	return new SpringApplicationRunListeners(logger, getSpringFactoriesInstances(
				SpringApplicationRunListener.class, types, this, args));
}
```
又出现了`getSpringFactoriesInstances()`方法，关于该方法我另外一篇文章([Spring.Factories机制源码级分析]({{site.url}}/spring-2.0.0-spring-factories))有阐述，这里不再述说。

这次`getSpringFactoriesInstances`方法传入的是`SpringApplicationRunListener`接口，通过debug跟踪，该方法的返回值为：

- org.springframework.boot.context.event.EventPublishingRunListener

继续进入该类源码：

```
public class EventPublishingRunListener implements SpringApplicationRunListener, Ordered {

	private final SpringApplication application;

	private final String[] args;

	private final SimpleApplicationEventMulticaster initialMulticaster;

	public EventPublishingRunListener(SpringApplication application, String[] args) {
		this.application = application;
		this.args = args;
		this.initialMulticaster = new SimpleApplicationEventMulticaster();
		for (ApplicationListener<?> listener : application.getListeners()) {
			this.initialMulticaster.addApplicationListener(listener);
		}
	}

	............
}
```
看到构造方法没！有没有发现了什么？

里面有四个逻辑操作，其中最核心的是

```
//初始化了一个【广播器】
this.initialMulticaster = new SimpleApplicationEventMulticaster();
//看到没【application.getListeners()】，这个不就是我们上一篇文章里，创建Application的时候复制的吗，在这里进行了关联，把所有的监听器都赋值给了这个【广播器】
for (ApplicationListener<?> listener : application.getListeners()) {
	this.initialMulticaster.addApplicationListener(listener);
}		
```
关于【广播器】这个类`SimpleApplicationEventMulticaster`，这里就不继续深入了，简单理解，这个类就是广播，spring的广播事件机制，都是基于该类实现的，有兴趣的可以研究一下

类路径：
- org.springframework.context.event.SimpleApplicationEventMulticaster 

类继承图
![](/images/blogs/SimpleApplicationEventMulticaster.png)

到此，我们已经得到`SpringApplicationRunListeners`对象，接下来是调用`starting()`;
```
listeners.starting();
```
照惯例，进源码看
```
//启动开始监听器
public void starting() {
	for (SpringApplicationRunListener listener : this.listeners) {
		listener.starting();
	}
}

//再进去就到了`EventPublishingRunListener`类，里面调用的就是`SimpleApplicationEventMulticaster`类的`multicastEvent`方法进行事件广播，这里不在深入，有兴趣可以继续看
public void starting() {
	this.initialMulticaster.multicastEvent(
				new ApplicationStartingEvent(this.application,this.args));
}
```

至此，这两行代码分析完毕，总结来说，这两天代码代表了spring的启动生命周期，每到一个生命周期，spring会调用相应的事件广播一次。

相关知识：
- [Spring-事件监听器器(Listener)]({{site.url}}/spring-2.0.1-Listener)


#### 第9~10行
```
//初始化默认应用参数类
ApplicationArguments applicationArguments = new DefaultApplicationArguments(
					args);
//根据监听器和应用参数类准备Spring环境
ConfigurableEnvironment environment = prepareEnvironment(listeners,
					applicationArguments);
```
这两行代码涉及到Spring Environment环境类，为节省篇幅，另开文章叙述

传送门：[Spring-环境类源码解析(Environment)]({{site.url}}/spring-2.0.1-Environment)















