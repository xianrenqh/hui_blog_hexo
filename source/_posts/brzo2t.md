---
title: Layui树形表格treetable的简单使用
urlname: brzo2t
date: '2022-05-24 10:01:18 +0000'
tags:
  - treetable
  - layui
  - 树形
categories: 学无止境
copyright_author_href: 'https://www.xiaohuihui.net'
copyright_url:
copyright_author:
cover:
---

```html
<body class="pear-container">
  <div class="layui-card">
    <div class="layui-card-body">
      <table id="admin_table" lay-filter="admin_table"></table>
    </div>
  </div>
  <script type="text/html" id="add_toolbar">
  <button class="pear-btn pear-btn-primary pear-btn-md {if !check_auth('{:__url('system/authrule/add')}')}layui-hide{/if}"
  data-open="{:__url('system/authrule/add')}"
  data-title="添加权限节点">
  <i class="layui-icon layui-icon-add-1"></i>
  添加权限节点
    </button>
  </script>

  <script type="text/html" id="isMenuTpl">
  {{#  if(d.is_menu =='1'){ }}
  <span class="layui-badge layui-bg-green"> 是 </span>
  {{#  } else { }}
  <span class="layui-badge layui-bg-danger"> 否 </span>
  {{#  } }}
  </script>

  <script type="text/html" id="statusTpl">
  {{#  if(d.status =='1'){ }}
  <span class="layui-badge layui-bg-green"> 是 </span>
  {{#  } else { }}
  <span class="layui-badge layui-bg-danger"> 否 </span>
  {{#  } }}
  </script>

  <script type="text/html" id="iconTpl">
  <span class="huicmf_icon"><i class="layui-inline layui-icon {{d.icon}}"></i></span>
  </script>

  <script type="text/html" id="barDemo">
  <button class="pear-btn pear-btn-primary pear-btn-sm" title="编辑"
  data-open="{:__url('system/authrule/edit')}?id={{d.id}}"
  data-title="编辑">
  <i class="layui-icon layui-icon-edit"></i>
    </button>
    <button class="pear-btn pear-btn-danger pear-btn-sm" title="删除"
    data-delete="{:__url('system/authrule/delete')}?id={{d.id}}"
    data-title="您确定要删除吗？" data-reload="2">
    <i class="layui-icon layui-icon-delete"></i>
    </button>
  </script>

 <!-- 这里引用layui等数据 -->
  <script>
    layui.use(['table', 'jquery','treetable'], function () {
      var $ = layui.jquery;
      var table = layui.table;
      let treetable = layui.treetable;
      var renderTable = function(){
        layer.load(2);  //加载层
        treetable.render({
          treeColIndex: 1,
          treeSpid: 0,
          treeIdName: 'title',
          treePidName: 'pid',
          treeDefaultClose: false,
          toolbar: '#add_toolbar',
          defaultToolbar: [{
            layEvent: 'refresh',
            icon: 'layui-icon-refresh',
            title: '刷新'
          }, 'filter', 'exports'],
          elem: '#admin_table',
          url: "{:__url('system/authrule/index')}",
          id: 'testReload',
          done: function () {
            layer.closeAll('loading');
          },
          page: false,
          cols: [
            [
              {field: 'id', title: 'ID', width: 60, align: 'center', sort: true}
              , {field: 'title_tree', title: '节点名称',width:200}
              , {field: 'node', title: '节点规则',width:300}
              , {field: 'icon', title: '图标',width:80, templet: '#iconTpl', align: 'center'}
              , {field: 'is_menu', title: '是否菜单', templet: '#isMenuTpl', align: 'center',width:90}
              , {field: 'status', title: '状态', templet: '#statusTpl', align: 'center'}
              , {field: 'create_time', title: '添加时间'}
              , {fixed: 'right', title: '操作', toolbar: '#barDemo', align: 'center', width: 130}
            ]
          ]
        });
      }
      renderTable();

      //监听行工具事件
      table.on('toolbar(admin_table)', function (obj) {
        if (obj.event === 'refresh') {
          renderTable();
        }
      });

    });
  </script>
</body>
</html>

```
