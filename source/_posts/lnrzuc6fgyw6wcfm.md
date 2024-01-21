---
title: layui流加载 终止flow.load()，执行第二个流加载时禁止上一个流加载执行
urlname: lnrzuc6fgyw6wcfm
date: '2024-01-16 03:08:13 +0000'
tags:
  - layui
  - 流加载
  - flow
categories: 学无止境
copyright_author_href: 'https://www.xiaohuihui.cc'
copyright_url:
copyright_author:
cover:
---

### [layui](https://so.csdn.net/so/search?q=layui&spm=1001.2101.3001.7020)流加载 终止 flow.load()

思路是当点击事件触发时，把 ul 移除，绑定的事件移除，重新加载 ul，相当于把之前的删除重新加载了[flow](https://so.csdn.net/so/search?q=flow&spm=1001.2101.3001.7020).load。

```nginx
主要代码是：
$("#demo").remove();
$(document).unbind(); //把容器的事件解除绑定
```

```nginx
//初始化流加载
getAjaxData();
function getAjaxData (){
	layui.use('flow', function(){
	  var $ = layui.jquery; //不用额外加载jQuery，flow模块本身是有依赖jQuery的，直接用即可。
	  var flow = layui.flow;
	  flow.load({
	    elem: '#demo' //指定列表容器
	    ,done: function(page, next){ //到达临界点（默认滚动触发），触发下一页
	      var lis = [];
	      //以jQuery的Ajax请求为例，请求下一页数据（注意：page是从2开始返回）
	      $.get('/api/list?page='+page, function(res){
	        //假设你的列表返回在data集合中
	        layui.each(res.data, function(index, item){
	          lis.push('<li>'+ item.title +'</li>');
	        });
	        //执行下一页渲染，第二参数为：满足“加载更多”的条件，即后面仍有分页
	        //pages为Ajax返回的总页数，只有当前页小于总页数的情况下，才会继续出现加载更多
	        next(lis.join(''), page < res.pages);
	      });
	    }
	  });
	});
}
//点击事件
$('#btn').click(function () {
    $("#demo").remove();
    $(document).unbind();
    $('#tab-active').append('<ul id="demo"></ul>');
    getAjaxData();	//执行流加载函数
});

```
