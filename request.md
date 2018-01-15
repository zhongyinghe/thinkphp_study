1、获取url数据
```php
$request = Request::instance();
//当前域名
echo 'domain: '.$request->domain().'<br>';
//获取当前入口文件
echo 'file: '.$request->baseFile().'<br>';
//获取当前url
echo 'url: '.$request->url().'<br>';
//获取当前完整url地址
echo 'url width domain: '.$request->url(true).'<br>';
// 获取当前URL地址 不含QUERY_STRING
echo 'url without query: '.$request->baseUrl().'<br>';
//获取root地址
echo 'root: '.$request->root().'<br>';
//包含域名的root
echo 'root with domain: '.$request->root(true).'<br>';
//获取PATHINFO信息
echo 'pathinfo: '.$request->pathinfo().'<br>';
//PATHINFO不含后缀
echo 'pathinfo: '.$request->path().'<br>';
//获取url地址中的后缀信息
echo 'ext: '.$request->ext().'<br>';
```
2、获取模块信息
```php
$request = Request::instance();
echo '当前模块名称是'.$request->module().'<br>';
echo '当前控制器名称是'.$request->controller().'<br>';
echo '当前操作名称是'.$request->action().'<br>';
echo '路由信息：';
dump($request->route());
echo '调度信息: ';
dump($request->dispatch());
```
3、获取param
```php
$request = Request::instance();
echo '请求方法:'.$request->method().'<br>';
echo '资源类型:'.$request->type().'<br>';
echo '访问ip地址:'.$request->ip().'<br>';
echo '是否Ajax请求:'.var_export($request->isAjax(), true).'<br>';
echo '请求参数:';
dump($request->param());
echo '请求参数,仅包含name';
dump($request->only(['name']));
echo '请求参数,排除name';
dump($request->except(['name']));
```
