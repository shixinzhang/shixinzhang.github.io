---
layout: post
title: 《Java 开发接口》遇到的问题及解决方法记录
categories: JavaWeb
description: 后端开发浅学笔记
keywords: 
---

购买的阿里云得利用起来了。

- [SpringMVC教程](https://www.yiibai.com/spring_mvc/)



2019年4月7日星期日





# 12.SpringMVC 写接口小结



SpringMVC 写一个简单接口已经 ok 了，需要配置的还是挺多的，小结一下：

1. pom 文件加配置就不说了，第一步首先是创建 applicationContext 和 dispatcher-servlet 文件，具体作用需要查一下，貌似 applicationContext 是配置 jdbc 驱动和数据库信息
2. dispatcher-servlet 里配置要扫描包的路径，和开启注解等
3. web.xml 里的配置是为了把刚才创建的 applicationContext 和 dispatcher-servlet  派上用场
4. 写 Controller，类上的注解 RequestMapping 值是访问 api 的前缀
5. 创建 dao 接口，定义要做的功能，增删改查
6. 创建 dao 实现，类注解用 @Repository，用 JdbcTemplate 做 sql 的执行器
7. 最后在 Controller 里写具体增删改查接口，注解方法需要使用 @RequestMapping 和 @ResponseBody 注解 

# 11.Controller 配置的路径，访问的却是  jsp 文件？？





Controller 直接配置 ``RequestMapping``，返回的 jsp 文件名就是要访问的：



```

@Controller
@RequestMapping("/developer")
public class DeveloperController {

    @RequestMapping(value = "/api/hello", method = RequestMethod.GET )
    public String hello() {
        //返回的是  jsp 的路径
        return "hello";
    }
}
```



如果在注解里加上 ``@ResponseBody``，就能返回 json 数据。？？感觉怪怪的



```

@RequestMapping(value = "/api/all", method = RequestMethod.GET)
@ResponseBody
public CommonModel getAllDevelopers() {
    List<DeveloperModel> allDevelopers = developerDao.getAllDevelopers();
    CommonModel commonModel = new CommonModel();
    if (allDevelopers != null && allDevelopers.size() > 0) {
        commonModel.setSuccess();
        commonModel.setData(allDevelopers);
    } else {
        commonModel.setFail();
    }

    return commonModel;
}
```



# 10.SpringMVC 配置出错，无法找到元素 'context:component-scan' 的声明



解决办法：修改补充 scheme 的内容。



>原因：spring启动的时候会通过xsd文件校验spring配置文件，找不到xsd文件就会报错



```

<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
         http://www.springframework.org/schema/mvc
          http://www.springframework.org/schema/mvc/spring-mvc.xsd
           http://www.springframework.org/schema/context
            http://www.springframework.org/schema/context/spring-context.xsd">
```





2019年4月6日星期六



# 9.Servlet RequestURI 和 RequestURL 区别



```

final String uri = request.getRequestURI();
final String url = request.getRequestURL().toString();

System.out.println(uri + "\n" + url);
```



第一个返回的是 path，第二个是完整地址：



```

/developer/api/developers

http://localhost:8080/developer/api/developers

```



# 8.查询结果里字段值反了



查询结果里的字段要和数据库里的顺序一致



# 7.``Servlet`` 访问 404





使用注解的话，检查有没有在 ``WebServlet`` 里配置 ``urlPatterns``，**这个参数表示支持访问的 path 名，支持多个！**

>别漏了前面的斜杠 ``/``



然后重启 server，访问  ``urlPatterns`` 里配置的路径即可。

# 6.部署 Servlet


# 5.新建没有 ``Servlet`` 选项



刷新 maven





# 4.运行 hello world 都有问题，几种情况



1. 只显示 Tomcat 猫 -》``shutdown.sh``

2. 无法找到服务，看看 log 里说啥，有可能端口占用了



>同时如果运行多个项目，需要关闭其他的。



# 3.Deploy 时找不到 war 包






去设置里允许 maven 自动导包：




或者手动同步一下 maven



# 2.Mysql密码



升级最新的不知道为什么启动有问题，还是使用旧的 5.7.X 吧



TYyxwt=9j+hP，新密码：root




# 1.Tomcat 启动失败






解决办法：



删掉 logs 目录下的那个没有权限的文件，反正也是日志文件，没关系。



>这个 ``catalina.out`` 里有报错信息，遇到问题可以去看看。