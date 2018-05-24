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
4、关于GET认识<br>
?号后面的才称之为GET变量<br>
所以PATHINFO模式下url中的路径是不能够作为GET变量的.例如:
```php
http://192.168.229.150/tp5/public/findId/123.html
```
该url中的123是无法使用get数组获取的

5、检验变量是否存在
```php
$hasId = Request::instance()->has('id');
$hasName = $hasName = Request::instance()->has('name');
```
但是为什么对get变量不能够这样呢?
```php
$hasId = Request::instance()->has('id', 'get');
$hasName = Request::instance()->has('nane', 'get');
```
这是因为PATHINFO模式下GET的认识(第4点);注意post的情况下是可以的.如:
```php
$hasId = Request::instance()->has('id', 'post');
$hasName = Request::instance()->has('nane', 'post');
```
6、获取变量，推荐使用如下方式:
```php
$name = Request::instance()->param('name');
```
这样可以同时处理pathinfo模式下的GET和POST<br>
其他获取请求参数的方式如下:
```php
Request::instance()->get('id'); // 获取某个get变量,pathinfo模式下是无效的
Request::instance()->get('name'); // 获取get变量
//上面可以使用如下代替
input('id');
input('name');
Request::instance()->post('name'); // 获取某个post变量
```
7、对输入变量进行过滤<br>
全局过滤:
```php
Request::instance()->filter('htmlspecialchars');
Request::instance()->filter(['strip_tags','htmlspecialchars']);
```
具体过滤:
```php
Request::instance()->get('name','','htmlspecialchars'); // 获取get变量 并用htmlspecialchars函数过滤
Request::instance()->param('username','','strip_tags'); // 获取param变量 并用strip_tags函数过滤
Request::instance()->post('name','','org\Filter::safeHtml'); // 获取post变量 并用org\Filter类的safeHtml方法过滤
Request::instance()->param('username','','strip_tags,strtolower'); // 获取param变量 并依次调用strip_tags、strtolower函数过滤
Request::instance()->post('email','',FILTER_VALIDATE_EMAIL);
```
类型正则过滤:
```php
input('get.id/d');//整形
input('post.name/s');//字符串
input('post.ids/a');//数组
Request::instance()->get('id/d');
Request::instance()->get('isHas/b');//布尔
Request::instance()->get('price/f');//浮点
```
8、获取部分变量或者排除部分变量<br>
获取部分变量:
```php
// 只获取当前请求的id和name变量
Request::instance()->only(['id','name']);
```
排除部分变量:
```php
// 排除id和name变量
Request::instance()->except(['id','name']);
```
9、请求类型判断
```php
// 是否为 GET 请求
if (Request::instance()->isGet()) echo "当前为 GET 请求";
// 是否为 POST 请求
if (Request::instance()->isPost()) echo "当前为 POST 请求";
// 是否为 PUT 请求
if (Request::instance()->isPut()) echo "当前为 PUT 请求";
// 是否为 DELETE 请求
if (Request::instance()->isDelete()) echo "当前为 DELETE 请求";
// 是否为 Ajax 请求
if (Request::instance()->isAjax()) echo "当前为 Ajax 请求";
// 是否为 Pjax 请求
if (Request::instance()->isPjax()) echo "当前为 Pjax 请求";
// 是否为手机访问
if (Request::instance()->isMobile()) echo "当前为手机访问";
// 是否为 HEAD 请求
if (Request::instance()->isHead()) echo "当前为 HEAD 请求";
// 是否为 Patch 请求
if (Request::instance()->isPatch()) echo "当前为 PATCH 请求";
// 是否为 OPTIONS 请求
if (Request::instance()->isOptions()) echo "当前为 OPTIONS 请求";
// 是否为 cli
if (Request::instance()->isCli()) echo "当前为 cli";
// 是否为 cgi
if (Request::instance()->isCgi()) echo "当前为 cgi";
```
10、请求伪装<br>
请求类型伪装:
```html
<form method="post" action="">
    <input type="text" name="name" value="Hello">
    <input type="hidden" name="_method" value="PUT" >
    <input type="submit" value="提交">
</form>
```
伪装请求的变量名可以通过应用配置来修改:
```php
// 表单请求类型伪装变量
'var_method'             => '_method',
```
ajax/pjax伪装
```
http://localhost/index?_ajax=1 
http://localhost/index?_pjax=1 
```
应用配置修改:
```php
// 表单ajax伪装变量
'var_ajax'               => '_ajax',
// 表单pjax伪装变量
'var_pjax'               => '_pjax',
```
11、获取头信息
```php
$info = Request::instance()->header();
$agent = Request::instance()->header('user-agent');
```
12、伪静态配置
```php
'url_html_suffix' => 'shtml'//设置一个后缀
// 多个伪静态后缀设置 用|分割
'url_html_suffix' => 'html|shtml|xml' 
// 关闭伪静态后缀访问
'url_html_suffix' => false,
```
13、操作方法参数绑定
```php
public function read($id) {
	return 'id='.$id;
}

public function archive($year='2018', $month="01") {
	return 'year='.$year.'&month='.$month;
}
```
按照名称绑定:<br>
这样访问:
```
http://serverName/index.php/index/blog/read/id/5
http://serverName/index.php/index/blog/archive/year/2018/month/06
```
按照顺序绑定:<br>
配置文件配置为:
```php
// URL参数方式改成顺序解析
'url_param_type'         => 1,
```
访问是这样:
```
http://serverName/index.php/index/blog/read/5
http://serverName/index.php/index/blog/archive/2018/06
```
14、请求缓存
```php
/ 定义GET请求路由规则 并设置3600秒的缓存
Route::get('new/:id','News/read',['cache'=>3600]);
```
15、设置thinkphp5.0跨域请求<br>
在入口文件中，或者在基础控制器中如下设置
```
header('Access-Control-Allow-Origin: *');
header('Access-Control-Allow-Headers: Authorization, Content-Type, If-Match, If-Modified-Since, If-None-Match, If-Unmodified-Since, X-Requested-With');
header('Access-Control-Allow-Methods: GET, POST, PATCH, PUT, DELETE');
header('Access-Control-Max-Age: 3600');
```
