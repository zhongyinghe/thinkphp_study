1、路由模式有哪三种?它们如何配置?<br>
普通模式,配置为:
```php
'url_route_on'  =>  false,
```
混合模式,配置为:
```php
'url_route_on'  =>  true,
'url_route_must'=>  false,
```
强制模式,配置为:
```php
'url_route_on'  		=>  true,
'url_route_must'		=>  true,
```
在强制模式下必须配置路由才能访问，否则会报错<br>

2、如何进行动态注册路由?<br>
注册示例:
```php
use think\Route;
// 注册路由到index模块的News控制器的read操作
Route::rule('new/:id','index/News/read');

//指定请求方法的注册
Route::rule('new/:id','News/update','POST');

//批量注册
Route::rule(['new/:id'=>'News/read','blog/:name'=>'Blog/detail']);
Route::get(['new/:id'=>'News/read','blog/:name'=>'Blog/detail']);
Route::post(['new/:id'=>'News/update','blog/:name'=>'Blog/detail']);
```

3、如何进行路由表达式的设置?
```php
'/' => 'index', // 首页访问路由
'my'        =>  'Member/myinfo', // 静态地址路由
'blog/:id'  =>  'Blog/read', // 静态地址和动态地址结合
'new/:year/:month/:day'=>'News/read', // 静态地址和动态地址结合
':user/:blog_id'=>'Blog/read',// 全动态地址
```
可选匹配:
```
'blog/:year/[:month]'=>'Blog/archive',
```
完全匹配:
```
'new/:cate$'=> 'News/category',
//或者开启完全匹配模式
// 开启路由定义的全局完全匹配
'route_complete_match'  =>  true,//在应用配置文件中设置
//对个别路由不想完全匹配的
Route::rule('new/:id','News/read','GET|POST',['complete_match' => false]);
```

