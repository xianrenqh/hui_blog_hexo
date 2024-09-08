---
title: 前后端分离和不分离的区别是什么
urlname: nufoqv
date: '2022-04-25 07:22:30 +0000'
tags:
  - 分离
  - 前后端
categories: 学无止境
copyright_author_href: 'https://www.xiaohuihui.net'
---

> <font style="color:rgb(102, 102, 102);">区别：前后端不分离中，前端页面看到的效果都是由后端控制，由后端渲染页面或重定向，即后端需要控制前端的展示，前端与后端的耦合度很高。前后端分离中，后端仅返回前端所需的数据，不再渲染 HTML 页面，不再控制前端的效果，前端与后端的耦合度相对较低。</font>

![](https://cdn.nlark.com/yuque/0/2022/png/27022430/1650871464824-462304b8-4353-4b00-8840-0d3fba42a74c.png)

# <font style="color:rgb(68, 68, 68);">一、前后端分离的概念</font>

## <font style="color:rgb(68, 68, 68);">1、前后端分离</font>

- <font style="color:rgb(68, 68, 68);">前后端分离是一种架构模式，说通俗点就是后端项目里面看不到页面</font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">（JSP | HTML）</font><font style="color:rgb(68, 68, 68);">，后端给前端提供接口，前端调用后端提供的 </font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">REST</font><font style="color:rgb(68, 68, 68);"> 风格接口就行，前端专注写页面</font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">（html|jsp）</font><font style="color:rgb(68, 68, 68);">和渲染</font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">（JS|CSS|各种前端框架）</font><font style="color:rgb(68, 68, 68);">；后端专注写代码就行。</font>
- <font style="color:rgb(68, 68, 68);">前后端分离的核心：</font>**<font style="color:rgb(68, 68, 68);">后台提供数据，前端负责显示</font>**

### <font style="color:rgb(68, 68, 68);">1、软件架构模式</font>

<font style="color:rgb(68, 68, 68);">最熟悉</font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">MVC</font><font style="color:rgb(68, 68, 68);">设计模式，</font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">Model—View-Controller</font><font style="color:rgb(68, 68, 68);"> 模型-视图-控制器</font>

- <font style="color:rgb(68, 68, 68);">它是怎么工作的？通俗来说：你在页面输入一个网址</font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">（请求-Request）</font><font style="color:rgb(68, 68, 68);">，这个网址跑到哪里去了呢？就去调用接口  
  </font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">（REST 风格）</font><font style="color:rgb(68, 68, 68);">，这个接口其实就是访问后端的一段代码（方法），后端有很多方法。</font>
- <font style="color:rgb(68, 68, 68);">如何确定访问的是哪个方法？就是接口定义好的，比如：</font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">177.25.26.7/idp/user/login</font><font style="color:rgb(68, 68, 68);">，这里面的</font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">idp</font><font style="color:rgb(68, 68, 68);">就表示一  
  </font><font style="color:rgb(68, 68, 68);">个服务(一个工程)，</font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">user</font><font style="color:rgb(68, 68, 68);">表示一个类，</font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">login</font><font style="color:rgb(68, 68, 68);">表示具体要调用的那个方法，你一旦回车，就直接调用了后端这个方法，后端这个方法就去数据库</font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">（MySQL|Oracle|其他）</font><font style="color:rgb(68, 68, 68);">取数据，数据库表中每一行数据就表示一个对象，也就是一个</font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">JavaBean</font><font style="color:rgb(68, 68, 68);">，最终用</font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">pojo</font><font style="color:rgb(68, 68, 68);">方式存到集合框架 </font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">（List|Map|Set|等）</font><font style="color:rgb(68, 68, 68);">中，方法把这个集合返回，那么调用这个接口的结果就是会响</font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">（Response）</font><font style="color:rgb(68, 68, 68);">给你一个结果集，前端就拿到了这个数据，然后通过页面</font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">（html|Jsp）</font><font style="color:rgb(68, 68, 68);">展现出来，这个用户看到的就是</font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">View</font><font style="color:rgb(68, 68, 68);">层做的事情。</font>

## <font style="color:rgb(68, 68, 68);">2、前后端分离的原因</font>

<font style="color:rgb(68, 68, 68);">在以前，听说 </font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">TDD (Test-driven development，测试驱动开发)</font><font style="color:rgb(68, 68, 68);"> 可以改善代码的质量，我们便实施了 </font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">TDD</font><font style="color:rgb(68, 68, 68);">；接着，听说 </font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">BDD (Behavior-driven development，行为驱动开发)</font><font style="color:rgb(68, 68, 68);"> 可以交付符合业务需求的软件，我们便实施了 BDD；后来，听说 </font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">DDD (Domain-driven design，领域驱动设计)</font><font style="color:rgb(68, 68, 68);"> 可以分离业务代码与基础代码，我们便实施了 </font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">DDD</font><font style="color:rgb(68, 68, 68);">。今天，听说了前后端分离很流行，于是我们就实施了前后端分离——这就是传说中的 </font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">HDD（Hype-driven Development，热闹驱动开发）</font><font style="color:rgb(68, 68, 68);">。</font>

**<font style="color:rgb(68, 68, 68);">过程</font>**<font style="color:rgb(68, 68, 68);">： </font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">TDD -》 BDD -》 DDD =》 HDD</font>

## <font style="color:rgb(68, 68, 68);">3、前后端分离的优点</font>

<font style="color:rgb(68, 68, 68);">前后端分离则可以很好的解决前后端分工不均的问题，将更多的交互逻辑分配给前端来处理，而后端则可以专注于其本职工作，比如提供</font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">API</font><font style="color:rgb(68, 68, 68);">接口，进行权限控制以及进行运算工作。而前端开发人员则可以利用</font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">nodejs</font><font style="color:rgb(68, 68, 68);">来搭建自己的本地服务器，直接在本地开发，然后通过一些插件来将</font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">api</font><font style="color:rgb(68, 68, 68);">请求转发到后台，这样就可以完全模拟线上的场景，并且与后台解耦。前端可以独立完成与用户交互的整一个过程，两者都可以同时开工，不互相依赖，开发效率更快，而且分工比较均衡。</font>

**<font style="color:rgb(68, 68, 68);">总结优点如下：</font>**

- <font style="color:rgb(68, 68, 68);">提升开发效率</font>
- <font style="color:rgb(68, 68, 68);">完美应对复杂多变的前端需求</font>
- <font style="color:rgb(68, 68, 68);">增强代码可维护性</font>

# <font style="color:rgb(68, 68, 68);">二、前后端分离和前后端不分离的区别</font>

## <font style="color:rgb(68, 68, 68);">1、前后端不分离</font>

- <font style="color:rgb(68, 68, 68);">在前后端不分离的应用模式中，前端页面看到的效果都是由后端控制，由后端渲染页面或重定向，也就是后端需要控制前端的展示，前端与后端的耦合度很高。</font>
- <font style="color:rgb(68, 68, 68);">这种应用模式比较适合纯网页应用，但是当后端对接</font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">App</font><font style="color:rgb(68, 68, 68);">时，</font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">App</font><font style="color:rgb(68, 68, 68);">可能并不需要后端返回一个</font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">HTML</font><font style="color:rgb(68, 68, 68);">网页，而仅仅是数据本身，所以后端原本返回网页的接口不再适用于前端</font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">App</font><font style="color:rgb(68, 68, 68);">应用，为了对接</font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">App</font><font style="color:rgb(68, 68, 68);">后端还需再开发一套接口。</font>

## <font style="color:rgb(68, 68, 68);">2、前后端分离</font>

    - <font style="color:rgb(68, 68, 68);">在前后端分离的应用模式中，后端仅返回前端所需的数据，不再渲染</font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">HTML</font><font style="color:rgb(68, 68, 68);">页面，不再控制前端的效果。至于前端用户看到什么效果，从后端请求的数据如何加载到前端中，都由前端自己决定，网页有网页的处理方式，</font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">App</font><font style="color:rgb(68, 68, 68);">有</font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">App</font><font style="color:rgb(68, 68, 68);">的处理方式，但无论哪种前端，所需的数据基本相同，后端仅需开发一套逻辑对外提供数据即可。</font>
    - <font style="color:rgb(68, 68, 68);">在前后端分离的应用模式中 ，前端与后端的耦合度相对较低。</font>
    - <font style="color:rgb(68, 68, 68);">在前后端分离的应用模式中，我们通常将后端开发的每个视图都称为一个接口，或者</font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">API</font><font style="color:rgb(68, 68, 68);">，前端通过访问接口来对数据进行增删改查。</font>
