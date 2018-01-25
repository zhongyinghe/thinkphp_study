1、模板标签<br>
应用配置文件中配置:
```php
'template'               => [
    // 模板引擎类型 支持 php think 支持扩展
    'type'         => 'Think',
    // 模板路径
    'view_path'    => '',
    // 模板后缀
    'view_suffix'  => 'html',
    // 模板文件名分隔符
    'view_depr'    => DS,
    // 模板引擎普通标签开始标记
    'tpl_begin'    => '<{',
    // 模板引擎普通标签结束标记
    'tpl_end'      => '}>',
    // 标签库标签开始标记
    'taglib_begin' => '<',
    // 标签库标签结束标记
    'taglib_end'   => '>',
],
```
模板输出:
```php
<{$name}>---<{$email}><br>
<eq name="name" value="abc">
	相等
<else/>
	不相等
</eq>
```
2、获取请求参数
```
<{$Request.get.id}>
```
3、使用函数
```
//使用函数
<{$addTime|date="Y-m-d H:i;s",###}>
<{$username|substr=0,5}>
```
4、使用默认值
```
<{$nickname|default="这家伙很懒，什么也没有留下"}>
```
5、使用运算符
```
<{$score+10}>
<{$score-10}>
<{$score*10}>
<{$score/10}>
<{$score%9}>
//三元运算符
<{$status ? "ok":"no"}>
```
6、原样输出
```
<literal>
    Hello,{$name}！
</literal>
```
7、模板注释
```
</*这是原样输出*/>
</*这是原
样输出*/>
<//这是模板注释>
```
8、模板布局<br>
定义布局文件:
```
<include file="public/header" />
 {__CONTENT__}
<include file="public/footer" />
```
使用布局文件：
```
<layout name="layout/layout">
<span>我是测试模板布局!</span>
```
9、模板继承<br>
base.html文件(public/base.html):
```
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8">
<title><block name="title">标题</block></title>
</head>
<body>
<block name="menu">菜单</block><br>
<block name="left">左边分栏</block><br>
<block name="main">主内容</block><br>
<block name="right">右边分栏</block><br>
<block name="footer">底部</block>
</body>
</html>
```
模板文件内存:
```
<extend name="public:base" />
<block name="title"><{$title}></block>
<block name="menu">
<a href="/" >首页</a>
<a href="/info/" >资讯</a>
<a href="/bbs/" >论坛</a>
</block>
<block name="left"></block>
<block name="main">
<volist name="list" id="vo">
<a href="/new/{$vo.id}"><{$vo.title}></a><br/>
 <{$vo.content}>
</volist>
</block>
<block name="right">
 最新资讯：
<volist name="news" id="new">
<a href="/new/{$new.id}"><{$new.title}></a><br/>
</volist>
</block>
<block name="footer">
{__block__}
 @ThinkPHP 版权所有
</block>
```
