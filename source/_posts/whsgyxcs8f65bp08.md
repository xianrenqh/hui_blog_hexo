---
title: 结合ajax完成layui的分页
urlname: whsgyxcs8f65bp08
date: '2023-02-10 09:04:24 +0000'
tags: []
categories: 学无止境
copyright_author_href: 'https://www.xiaohuihui.cc'
copyright_url:
copyright_author:
cover:
---

本文为大家介绍[layui](https://so.csdn.net/so/search?q=layui&spm=1001.2101.3001.7020)的分页使用教程，步骤详细，欢迎学习。 ~
官网地址：[https://layui.gitee.io/v2/docs/modules/laypage.html](https://layui.gitee.io/v2/docs/modules/laypage.html)
laypage 的使用非常简单，指向一个用于存放分页的容器，通过服务端得到一些初始值，即可完成分页渲染：

一、导入相关 JS

```
<script type="text/javascript" src="js/jquery.min.js"></script>
<script type="text/javascript" src="js/layui.js"></script>
```

二、选择容器

```
<div id="test1"></div>
```

三、初始化分页

```
layui.use('laypage', function(){
        $.ajax({
            type:"POST",
            url:"",
            dataType:"json",
            data:{},
            success:function (data) {
                //执行一个laypage实例
                laypage.render({
                    elem: 'test1' //注意，这里的 test1 是 ID，不用加 # 号
                    ,count: data.totalRow //数据总数，从服务端得到
                    ,limit:4 //每页条数
                    ,prev:"🔼"
                    ,next:"🔽"
                    ,theme: '#FF5722'
                    ,layout: ['count', 'prev', 'page', 'next']
                    ,jump: function(obj, first){
                        //obj包含了当前分页的所有参数，比如：
                        //  console.log(obj.curr); //得到当前页，以便向服务端请求对应页的数据。
                        //  console.log(obj.limit); //得到每页显示的条数
                        $.ajax({
                            type:"POST",
                            url:"",
                            dataType:"json",
                            data:{"courseId":courseId,page:obj.curr,pageSize: 4},
                            success:function (data) {
                                var dataList = data.list;
                                for (var i = 0; i < dataList.length; i++) {
                                    classRoomName = dataList[i].name;
                                    playSource = dataList[i].device_preview_rtmp;
                                }
                            }
                        })
                    }
                });
            }
        });
});
```

注：以上就是结合[ajax](https://so.csdn.net/so/search?q=ajax&spm=1001.2101.3001.7020)完成 layui 的分页教程，非常简单，其他分页样式及参数介绍在官网中有很详细的介绍可以去学习。
