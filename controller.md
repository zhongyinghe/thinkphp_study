1、如果想对模块下的控制器进行统一控制，该如何处理呢?
```php
<?php 
namespace app\index\controller;
use think\Controller;

class Base extends Controller {
	public function _initialize() {
		
	}
}
```
2、有哪两种渲染模板的方法?<br>
第一种:
```php
namespace app\index\controller;
use think\View;
class Index 
{
    public function index()
    {
        $view = new View();
        return $view->fetch('index');
    }
}
//或者
namespace app\index\controller;
class Index 
{
    public function index()
    {
        return view('index');
    }
}
```
第二种:
```php
namespace app\index\controller;

use think\Controller;

class Index extends Controller
{
    public function index()
    {
        // 获取包含域名的完整URL地址
        $this->assign('domain',$this->request->url(true));
        return $this->fetch('index');
    }
}
```

3、如何进行前置操作设置?
```php
amespace app\index\controller;
use think\Controller;
class Index extends Controller
{
    protected $beforeActionList = [
        'first',
        'second' =>  ['except'=>'hello'],
        'three'  =>  ['only'=>'hello,data'],
    ];
    
    protected function first()
    {
        echo 'first<br/>';
    }
```

4、成功和错误的页面跳转该如何设置?
```php
$this->success('成功','index/blog/data');
$this->error("失败");
//直接跳转为
$this->redirect('index/blog/data');
$this->redirect('News/category', ['cate_id' => 2], 302, ['data' => 'hello']);
```
如何配置跳转的页面呢?
```php
//可以这样配置
//默认错误跳转对应的模板文件
'dispatch_error_tmpl' => APP_PATH . 'tpl/dispatch_jump.tpl',
//默认成功跳转对应的模板文件
'dispatch_success_tmpl' => APP_PATH . 'tpl/dispatch_jump.tpl',
//或者
//默认错误跳转对应的模板文件
'dispatch_error_tmpl' => 'public/error',
//默认成功跳转对应的模板文件
'dispatch_success_tmpl' => 'public/success',
```
5、什么是空操作?<br>
就是找不到方法时调用的方法
```php
public function _empty($name) {
}
```

6、什么是空控制器?<br>
当系统找不到控制器时调用指定的控制器<br>
配置文件指定空控制器名称
```php
// 更改默认的空控制器名
'empty_controller'      => 'MyError',
```
具体代码为:
```php
namespace app\index\controller;
use think\Request;
class MyError 
{
    public function index(Request $request)
    {
        //根据当前控制器名来判断要执行那个城市的操作
        $cityName = $request->controller();
        return $this->city($cityName);
    }
    
    //注意 city方法 本身是 protected 方法
    protected function city($name)
    {
        //和$name这个城市相关的处理
         return '当前城市' . $name;
    }
}
```


