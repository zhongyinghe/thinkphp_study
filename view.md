1、模板配置,应用配置文件中
```php
'template'               => [
    // 模板引擎类型 支持 php think 支持扩展
    'type'         => 'Think',
    // 模板路径
    'view_path'    => './template/',
    // 模板后缀
    'view_suffix'  => 'html',
    // 模板文件名分隔符
    'view_depr'    => DS,
    // 模板引擎普通标签开始标记
    'tpl_begin'    => '{',
    // 模板引擎普通标签结束标记
    'tpl_end'      => '}',
    // 标签库标签开始标记
    'taglib_begin' => '{',
    // 标签库标签结束标记
    'taglib_end'   => '}',
],
```
2、视图示例化
```php
 // 渲染模板输出
return $this->fetch('hello',['name'=>'thinkphp']);
//助手函数
return view('hello',['name'=>'thinkphp']);
```
3、模板赋值和模板渲染
```php
//方式一
public function index() {
  $this->assign('name', 'ThinkPHP');
  $this->assign('email', 'thinkphp@qq.com');
  return $this->fetch();
}

//方式二
public function edit() {
  $this->assign([
    'name' => 'edit',
    'email' => 'edit@thinkphp.com'
    ]);
  return $this->fetch('edit');
}

//方式三
public function show() {
  return $this->fetch('show', [
    'abc' => 'hello, abc',
    'bbs' => 'hello, bbs',
    ]);
}

//跨控制器渲染模板
public function blog() {
  return $this->fetch('blog/index');
}

//跨模块渲染模板
return $this->fetch('admin@member/edit');
```
