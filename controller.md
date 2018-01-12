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
