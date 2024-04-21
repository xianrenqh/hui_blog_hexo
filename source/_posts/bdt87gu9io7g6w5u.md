---
title: 使用eacharts画行政区域规划图
urlname: bdt87gu9io7g6w5u
date: '2024-04-19 07:11:26 +0000'
tags: []
categories: []
---

tags: [eacharts，行政区域，规划图]
categories: 学无止境
copyright_author_href: https://www.xiaohuihui.cc
copyright_url:
copyright_author: 小灰灰
cover: [https://cdn.nlark.com/yuque/0/2024/png/27022430/1713511104993-e973c846-fd56-4cfb-a732-a9779e788c8e.png#averageHue=%233f617b&clientId=u657576c4-b898-4&from=paste&height=480&id=u703bdffe&originHeight=480&originWidth=868&originalType=binary%E2%88%B6=1&rotation=0&showTitle=false&size=90821&status=done&style=none&taskId=ubac07a77-db62-4e93-a048-a3ef0ce9eb0&title=&width=868](https://cdn.nlark.com/yuque/0/2024/png/27022430/1713511104993-e973c846-fd56-4cfb-a732-a9779e788c8e.png#averageHue=%233f617b&clientId=u657576c4-b898-4&from=paste&height=480&id=u703bdffe&originHeight=480&originWidth=868&originalType=binary%E2%88%B6=1&rotation=0&showTitle=false&size=90821&status=done&style=none&taskId=ubac07a77-db62-4e93-a048-a3ef0ce9eb0&title=&width=868)

---

用 **Echarts** 无论是制作省份地图还是区县域地图，他们的步骤都是基本一样的。
下面就 利用 Echarts 简单绘制省份地图 的步骤与经验与各位分享一下。

![image.png](https://cdn.nlark.com/yuque/0/2024/png/27022430/1713511104993-e973c846-fd56-4cfb-a732-a9779e788c8e.png#averageHue=%233f617b&clientId=u657576c4-b898-4&from=paste&height=480&id=u703bdffe&originHeight=480&originWidth=868&originalType=binary∶=1&rotation=0&showTitle=false&size=90821&status=done&style=none&taskId=ubac07a77-db62-4e93-a048-a3ef0ce9eb0&title=&width=868)

## **1、准备工作**

- **1.1 下载 js 静态文件**

  - china.js
  - echarts.min.js

![image.png](https://cdn.nlark.com/yuque/0/2024/png/27022430/1713510784705-88bc093f-570c-41ee-b729-246c9541366d.png#averageHue=%233d4043&clientId=u657576c4-b898-4&from=paste&id=udb861626&originHeight=38&originWidth=260&originalType=url∶=1&rotation=0&showTitle=false&size=1703&status=done&style=none&taskId=uaf535bfe-2c52-427d-8e67-ab25fcc05a8&title=)

- **1.2 下载中国各省、各市的 .json 文件**

  - 省份或者地区的数据文件
  - 网址：[https://github.com/longwosion/geojson-map-china](https://github.com/longwosion/geojson-map-china)

## **2、获取省份数据**

- **2.1 第一步：获取 XX 省的地图 json 数据文件（例：河南省）（是以各省身份证号 前两位 开头命名的）**
- **2.2 第二步：将获取到的 JSON 文件 转换 成 js 文件（河南省：henan.js）**
- **2.3 第三步：修改转换后的 js 文件**
  - 打开 js 文件
  - 添加变量 xx (这里命名习惯为 ：（省名拼音小写+Json）例：henanJson) - var xx = (js 文件) - 例：
    var henanJson = {"type": "FeatureCollection",
    "cp":[118.8586,32.915],
    ........
    }

![image.png](https://cdn.nlark.com/yuque/0/2024/png/27022430/1713510951892-f7694d60-34a3-4f93-9b5c-8a6ea154222f.png#averageHue=%235b5e8b&clientId=u657576c4-b898-4&from=paste&height=429&id=u9b451a79&originHeight=429&originWidth=1184&originalType=binary∶=1&rotation=0&showTitle=false&size=374949&status=done&style=none&taskId=u01faf51c-565c-4fdd-b010-1ec72562547&title=&width=1184)

- 保存 js 文件

## **3、编写 HTML 代码**

- **3.1 在<head> </head>中引入 js 文件**

```
<script type="text/javascript" src="/static/js/echarts.min.js"></script>
<script type="text/javascript" src="/static/js/henan.js"></script>
```

- **3.2 在<body></body>中写入作图代码**

```
<div>
    {# 标记 #}
     <a class="btn btn-success btn-sm" >江苏省</a>
    {# 地图代码开始 #}
     <div class="x-body">
         <!-- 为ECharts准备一个具备大小（宽高）的Dom -->
         <div id="main" style="width: 949.75px;height:450px;"></div>
     </div>

      <script type="text/javascript">
               echarts.registerMap('henan', henanJson);

               // 基于准备好的dom，初始化echarts实例
               var myChart = echarts.init(document.getElementById('main'));

               var option = {
                      title: {
                          text: '',
                          left: 'center'
                      },
                      tooltip: {
                          trigger: 'item'
                      },
                      series: [
                          {
                              name: '河南省',
                              type: 'map',
                              mapType: '河南省',
                              roam: true, // 是否开启鼠标缩放和平移漫游
                              label: {
                                  show: true // 显示省份名称
                              },
                              data: [
                                  // 这里可以添加数据，例如：
                                   {name: '卫滨区', selected: true},
                                   //{name: '开封市', value: 200},
                                  // ...
                              ]
                          }
                      ]
                  };
            myChart.setOption(option);
       </script>
</div>
```

- **3.3 运行代码，就能看到结果**

![image.png](https://cdn.nlark.com/yuque/0/2024/png/27022430/1713510854010-6c0686b0-9d1b-4a2f-ab95-8af08fe59241.png#averageHue=%23f5f5f5&clientId=u657576c4-b898-4&from=paste&height=384&id=u3c3ace7a&originHeight=384&originWidth=473&originalType=binary∶=1&rotation=0&showTitle=false&size=39651&status=done&style=none&taskId=ua68ad514-05df-41da-a18e-c2979ee164f&title=&width=473)
如果各位对省图还需要其他显示功能，大家不妨访问 [Echarts](https://echarts.apache.org/) 的官网。
