---
layout: post
title: Spring 官方文档学习笔记
categories: JavaWeb
description: 记录学习Spring 官方文档的笔记
keywords: 后端学习 笔记 官方文档 JavaWeb
---



- https://docs.spring.io/spring/docs/current/spring-framework-reference/index.html



# IOC 容器



Thursday, August 1, 2019  【读完了 1.2.1】



>https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#spring-core



![container magic](https://docs.spring.io/spring/docs/current/spring-framework-reference/images/container-magic.png)




- 在 ``Spring`` 中，构成应用程序的核心，并由 ``Spring IoC`` 容器管理的对象称为 ``bean``

- ``Bean`` 及其之间的依赖关系反映在容器使用的配置元数据中

- ``BeanFactory`` 是定义了 bean 工厂基本功能的接口，被称作 "bean 容器"

- ``ApplicationContext`` 是   ``BeanFactory`` 的实现类

- 容器通过读取配置元数据获取有关要实例化，配置和组装的对象的指令，配置元数据以 XML，Java 注解或 Java代码表示

- 通常，不会在容器中配置细粒度域对象，因为 DAO 和业务逻辑通常负责创建和加载 细粒度的域对象




Friday, August 2, 2019  【1.3.1 读了一半】



- 让 ``bean`` 定义跨越多个 ``XML`` 文件会很有用

- 通常，每个单独的 ``XML`` 配置文件都代表架构中的一个逻辑层或模块（Controller Service Dao）

- 使用 ``<import/>``  标签可以导入其他配置文件里定义的 bean

- 通过 ``ApplicationContext``  可以读取 bean 配置文件和获取 bean 实例

- 实际上，您的应用程序代码根本不应该调用 ``getBean()`` 方法，避免依赖于Spring API，因为 Spring 为不同框架提供了不同的注入方式



```


<beans>
	<import resource="services.xml"/>
	<import resource="resources/messageSource.xml"/>
	<import resource="/resources/themeSource.xml"/>

	<bean id="bean1" class="..."/>
	<bean id="bean2" class="..."/>
</beans>
```



使用 ``Groovy`` 配置 bean:



```


beans {
	dataSource(BasicDataSource) {
		driverClassName = "org.hsqldb.jdbcDriver"
		url = "jdbc:hsqldb:mem:grailsDB"
		username = "sa"
		password = ""
		settings = [mynew:"setting"]
	}
	sessionFactory(SessionFactory) {
		dataSource = dataSource
	}
	myService(MyService) {
		nestedBean = { AnotherBean bean ->
			dataSource = dataSource
		}
	}
}
```



通过 ``ApplicationContext``  可以读取 bean 配置文件和访问 bean:



```


// create and configure beans
ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");

// retrieve configured instance
PetStoreService service = context.getBean("petStore", PetStoreService.class);

// use configured instance
List<String> userList = service.getUsernameList();
```



- 在基于XML的配置元数据中，使用 id 或者 name 属性或两者来座位 bean 的标识符

- 一个 bean 可以没有 id 和 name，但如果要通过名称引用该 bean（通过使用 ``ref`` 元素或 ``Service Locator``查找），则必须提供名称