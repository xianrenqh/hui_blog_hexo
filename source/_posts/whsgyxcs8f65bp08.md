---
title: 结合ajax完成layui的分页
urlname: whsgyxcs8f65bp08
date: '2023-02-10 09:04:24 +0000'
tags: []
categories: []
---

tags: []

categories: <font style="color:rgb(38, 38, 38);">学无止境</font>

copyright_author_href: https://www.xiaohuihui.<font style="color:rgb(38, 38, 38);">cc</font>

<font style="color:rgb(38, 38, 38);">copyright_url:  
</font><font style="color:rgb(38, 38, 38);">copyright_author: </font>

<font style="color:rgb(33, 37, 41);">cover:</font>

---

<font style="color:rgb(51, 51, 51);">  
</font><font style="color:rgb(77, 77, 77);">本文为大家介绍</font>[layui](https://so.csdn.net/so/search?q=layui&spm=1001.2101.3001.7020)<font style="color:rgb(77, 77, 77);">的分页使用教程，步骤详细，欢迎学习。 ~</font>  
<font style="color:rgb(77, 77, 77);">官网地址：</font>[https://layui.gitee.io/v2/docs/modules/laypage.html](https://layui.gitee.io/v2/docs/modules/laypage.html)  
<font style="color:rgb(77, 77, 77);">laypage 的使用非常简单，指向一个用于存放分页的容器，通过服务端得到一些初始值，即可完成分页渲染：</font>

<font style="color:rgb(77, 77, 77);"></font>

<font style="color:rgb(77, 77, 77);">一、导入相关 JS</font>

```plain
<script type="text/javascript" src="js/jquery.min.js"></script>
<script type="text/javascript" src="js/layui.js"></script>
```

<font style="color:rgb(77, 77, 77);">二、选择容器</font>

```plain
<div id="test1"></div>
```

<font style="color:rgb(77, 77, 77);">三、初始化分页</font>

```plain
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

<font style="color:rgb(77, 77, 77);">注：以上就是结合</font>[ajax](https://so.csdn.net/so/search?q=ajax&spm=1001.2101.3001.7020)<font style="color:rgb(77, 77, 77);">完成 layui 的分页教程，非常简单，其他分页样式及参数介绍在官网中有很详细的介绍可以去学习。</font>
