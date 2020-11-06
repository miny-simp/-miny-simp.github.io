---
title: js-layui问题合集
date: 2020-06-07 15:32:10
categories:
- web前端 
tags: 
- js
---
### layui radio赋值选中不了的问题
``` javascript
//   赋值以后，需要重新渲染form，加上代码 form.render();  就可以了
$("input[name='xmnd'][value='"+data.xmnd+"']").prop("checked", "checked");
form.render();
```
<!-- more -->
### layer弹出层的关闭问题
在执行完毕后关闭当前弹出层
1. layer.close(index) - 关闭特定层
``` javascript
//当你想关闭当前页的某个层时
var index = layer.open();
var index = layer.alert();
var index = layer.load();
var index = layer.tips();
//正如你看到的，每一种弹层调用方式，都会返回一个index
layer.close(index); //此时你只需要把获得的index，轻轻地赋予layer.close即可
 
//如果你想关闭最新弹出的层，直接获取layer.index即可
layer.close(layer.index); //它获取的始终是最新弹出的某个层，值是由layer内部动态递增计算的
 
//当你在iframe页面关闭自身时
var index = parent.layer.getFrameIndex(window.name); //先得到当前iframe层的索引
parent.layer.close(index); //再执行关闭   
```
2. layer.closeAll(type) - 关闭所有层
如果不指向层类型type的话，它会销毁掉当前页所有的layer层
``` javascript
// 关闭某个类型的层
layer.closeAll(); //疯狂模式，关闭所有层
layer.closeAll('dialog'); //关闭信息框
layer.closeAll('page'); //关闭所有页面层
layer.closeAll('iframe'); //关闭所有的iframe层
layer.closeAll('loading'); //关闭加载层
layer.closeAll('tips'); //关闭所有的tips层  
```
3. 关闭弹出层之后刷新父页面
window.parent.location.reload();//刷新父页面
``` javascript
success: function(data){
    var res = eval('(' + data + ')');
    if(res.status == '1'){
        layer.msg("添加成功！");
        layer.alert("添加成功！",function(){
            window.parent.location.reload();//刷新父页面
            parent.layer.close(index);//关闭弹出层
        });
    } else{
        layer.msg("添加失败！");
    }
},
```

### 分页封装
``` javascript
function stageInitPage(id, count, limit, curr){
	laypage.render({
		elem: id
		,count: count//数据总数
		,limit: limit//每页显示的条数。laypage将会借助 count 和 limit 计算出分页数。
		,groups: 3//连续出现的页码个数
    ,curr:curr
		// ,layout: ['count', 'prev', 'page', 'next', 'skip']
		,theme: '#007bff' 
		// ,prev:'<em>←</em>'
		// ,next:'<em>→</em>'
		,jump: function(obj, first){
			if(!first){
				// layer.msg('第 '+ obj.curr +' 页');
				sessionStorage.setItem('page-curr',obj.curr); // 当前页的缓存
				stageTypeList(); // 分页后需要刷新的方法
			}
		}
	});
}
```
### layer最大化、最小化、还原回调函数
``` javascript
var indexZp = layer.open({
  type: 2, 
  id: 'photoPlay',
  // title:false,
  title: data.mc,
  shade: 0.7,
  maxmin: true,
  //shadeClose: true, 
  fixed: false,
  area: ['800px', '500px'],
  // content: 'panorama/indexImg.html'
  content: 'zoomMarker/index.html',
  min: function() { 
    //点击最小化后的回调函数
  },
  full: function() { 
    //点击最大化后的回调函数
  },
  restore: function() { 
    //点击还原后的回调函数
  },
  end: function() { 
    // 关闭弹窗时的操作
  }
  });
  layer.full(indexZp);
})
```
### 时间控件闪现
初始化时间控件，点击触发时就一闪就消失了。这种情况主要是出现在弹出窗的时间控件上。
解决办法：初始化时加一个属性trigger: 'click',
``` javascript
laydate.render({
  elem: '#time',
  format: 'yyyy-MM-dd HH:mm:ss',
  type: 'datetime',
  position:'fixed',
  trigger: 'click',
});
```
### layer弹窗传值
layer.getChildFrame(selector, index) - 获取iframe页的DOM
当你试图在当前页获取iframe页的DOM元素时，你可以用此方法。
``` javascript
var wt = $('.box').width();
var ht = $('.box').height();
var indexZp = layer.open({
  type: 2,
  id: 'photoPlay',
  title: mc,
  shade: false,
  // shade: 0.7,
  maxmin: true,
  //shadeClose: true, 
  fixed: false,
  area: [wt + 'px', ht + 'px'],
  content: 'zoomMarker/index.html',
  success: function(layero, index){
    $('.layui-layer-max').attr('style','display:none;');
    var body = layer.getChildFrame('body', index);
    var iframeWin = window[layero.find('iframe')[0]['name']]; 
    body.find('#picPath').val(path);
    iframeWin.method();
  },
  end: function() {
  }
});
layer.full(indexZp);
```
### 前台分页
设置‘limit’和‘limits’的值
``` javascript
layui.table.render({
  elem: '#streamlineId',
  data: data,
  limit: 100,
  limits: [50, 100, 200],
  page: true,
  toolbar: true,
  toolbar: "#toolbarTpl",
  // size: "sm",
  cols: [
    [{ type: "checkbox", fixed: "left" },
      { type: "numbers",title: "序号",width: 60,align: "center" },
      { field: "bj", title: "部件", width: 60, align: "center" },
      { field: "bw", title: "部位", width: 90, align: "center" },
      { field: "qxms", title: "缺陷描述", width: 150 },
      { field: "flyj", title: "分类依据", width: 150 },
      { field: "qxfl", title: "缺陷分类", width: 90, align: "center" },
      { field: "bjzl", title: "部件种类", width: 90, align: "center" },
      { field: "wzqxms", title: "完整缺陷描述", width: 220 },
      { field: "remark", title: "备注" },
      { title: "操作", width: 80, align: "center", templet: "#operationTpl" }
    ]
  ],
  done: function (res, curr, count) {
    console.info(res, curr, count);
  }
});
```
### table组件edit编辑
layui table组件edit编辑事件获取单元格改之前的值,edit事件中给单元格赋值
``` javascript
// 添加 --- 单元格编辑监听
layui.table.on('edit(addWzTable)', function(obj){
  var value = obj.value //得到修改后的值
  ,data = obj.data //得到所在行所有键值
  ,field = obj.field; //得到字段
  var reg = /^[0-9]+([.]{1}[0-9]+){0,1}$/; // 数字验证

  // 获取单元格编辑之前td的选择器
  var selector = obj.tr.selector+' td[data-field="'+obj.field+'"] div';
  // 单元格编辑之前的值
  var oldtext = $(selector).text();

  var currentIndex = obj.tr[0].rowIndex + 1; // 当前tr的index
  if (!reg.test(value)) {
    layer.msg('请输入整数或小数！');
    // 验证不通过设置其值为空
    $('#addWzBox tr:nth-child('+currentIndex+')' + ' td[data-field="' + obj.field + '"] input').val('');
    $('#addWzBox tr:nth-child('+currentIndex+')' + ' td[data-field="' + obj.field + '"] div').text('');
    return false;
  }
});
```
### upload 在table头部工具栏放的按钮没有作用
原因：每次render表格的时候(reload也是render),会重新生成toolbar的html。
解决办法：
``` javascript
//按钮事件
ver fn= function(){
...
 })

//方法1  使用代理事件
$(document).on('click','#btn',fn);
//注意: #btn的dom上不能设置lay-event否则你绑定的事件不会被执行

//方法2  在table的done回调里重新给按钮绑定事件
table.render({
    //....
    ,done:function () {
        $('#btn').on('click',fn);
    }
    //..
}
// 注意: 如果你的table请求发生error 将不会执行done,  你可以选择改源码,table.js v2.4.5  694行后 增加 typeof options.done === 'function' && options.done(); 

// 方法3  如果是table监听的toolbar事件 则在事件reload后重新绑定事件
table.on('toolbar(tableLayFilter)', function(obj) {
     var layEvent = obj.event;
     if (layEvent === 'upload') {
         table.reload();
         //此处重新绑定事件
         $('#btn').on('click',fn);
     }
});
```
### select获取自定义属性方法
1. 定义html
``` html 
<select name="item_name" lay-filter="catalogSelect" id="item_name"></select>
```
2. js监听
``` javascript
// form监听
form.on('select(catalogSelect)', function(data){
  console.log(data.elem); //得到select原始DOM对象
  console.log(data.elem[data.elem.selectedIndex].title);
  $('#catalog_id').val(data.elem[data.elem.selectedIndex].title);
}); 
// select下拉框组装
function getZdlxSelect(data){
  var sb = [];
  for (var i = 0; i < data.length; i++){
    sb.push('<option value="'+data[i].code+'" title="'+data[i].id+'">'+data[i].catalog+'</option>');
  }
  $("#item_name").html(sb.join(''));
  form.render('select');
}
```
### 数据表格checkbox设置部分不可选
layui内置没有该功能，只能自己实现。
1. 使用templet实现
``` javascript
table.render({
  elem: '#junTable',
  url: '',
  cols: [[
    {
      templet: "#checkbd",
      title: "<input type='checkbox' name='siam_all' title='' lay-skin='primary' lay-filter='siam_all'> ",
      width: 60,
    }
    , {
      field: 'z_id',
      title: 'id'
    }
  ]],
  page: true,
  limit: 10
});
```
  html
  ``` html 
  <script type="text/html" id="checkbd">
    {{#  if (d.can_fabu === 1){ }}// 这里是判断要不要显示的条件
    <input type="checkbox" name="siam_one" title="" lay-skin="primary" data-id = "{{ d.z_id }}">
    {{#  } }}
  </script>
  <style>
  .laytable-cell-checkbox .layui-disabled.layui-form-checked i {
    background: #fff !important;
  }
  </style>
  ```
2. 全选功能
``` javascript
form.on("checkbox(siam_all)", function () {
  var status = $(this).prop("checked");
  $.each($("input[name=siam_one]"), function (i, value) {
      $(this).prop("checked", status);
  });
  form.render();
});
```
3. 获取选中数据
``` javascript
var ids = [];
$.each($("input[name=siam_one]:checked"), function (i, value) {
    ids[i] = $(this).attr("data-id"); 
});
```
4. 默认全选
``` javascript
$("input[name=siam_all]").attr("checked",'true');
$("input[name=siam_one]").attr("checked",'true');
form.render();
```
### 获取表格的全部数据
``` javascript
layui.table.cache["表格ID"] 
```
### 数据表格隐藏列（layui2.4版本）
``` javascript
{field:'cust_id', title: 'ID', hide:true}
```
### table的单元格改为select下拉框
``` javascript
{ field: "zyfs", title: "作业方式", minWidth: 100, templet: function(d){
  var s = [];
  var zyfsList = top.SystemDic.get("workType");
  if(zyfsList){
    s.push('<select id="'+d.currid+'" class="select-box-zyfs">');
    s.push('  <option value="">请选择</option>');
    for (var m = 0; m < zyfsList.length; m++) {
      s.push('<option value="'+zyfsList[m].item_code+'">'+zyfsList[m].item_name+'</option>');
    }
    s.push('</select>');
  }
  return s;
} },
```
``` css
/* 防止下拉框的下拉列表被隐藏---必须设置--- */        
.basic-info-box .layui-table-cell,
.basic-info-box .layui-table-box,
.basic-info-box .layui-table-body
  { 
  overflow: visible; 
} 
/* 使得下拉框与单元格刚好合适 */       
.basic-info-box td .layui-form-select {
  margin-top: -5px;
  margin-left: -14px;
  margin-right: -14px;
}
.basic-info-box td .layui-form-select .layui-input {
  border: none;
  height: 30px;
}
```
### 弹窗下的下拉选择框被遮挡或显示不全
这种写法存在的问题：导致弹窗的content的内容较多时，不会出现下拉框，内容溢出。
``` javascript
// 设置弹窗自定义样式
layer.open({
  skin: 'layer-ext-myskin',
});
```
``` css
/* 设置css */
body .layer-ext-myskin .layui-layer-content {
  overflow: visible;
}
```
### 未完待续
``` javascript

```