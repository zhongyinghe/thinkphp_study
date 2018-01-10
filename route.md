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
4、如何定义多个路由文件?
```php
// 定义路由配置文件（数组）
'route_config_file' =>  ['route', 'route1', 'route2'],
```
5、有哪3个变量规则?<br>
全局的:
```php
// 设置name变量规则（采用正则定义）
Route::pattern('name','\w+');
// 支持批量添加
Route::pattern([
    'name'  =>  '\w+',
    'id'    =>  '\d+',
]);
```
局部的:
```php
/ 定义GET请求路由规则 并设置name变量规则
Route::get('new/:name','News/read',[],['name'=>'\w+']);
```
完整的url规则:
```php
/ 定义GET请求路由规则 并设置完整URL变量规则
Route::get('new/:id','News/read',[],['__url__'=>'new\/\w+$']);
```
6、如何设置组合变量规则?
```php
'item-<name>-<id>' => ['Index/detail',['method' => 'get'],['name'=>'[a-zA-Z]+','id'=>'\d+']],
```

7、有哪些路由参数检测?<br>
请求类型:
```php
// 检测路由规则仅GET请求有效
Route::any('new/:id','News/read',['method'=>'get']);
// 检测路由规则仅GET和POST请求有效
Route::any('new/:id','News/read',['method'=>'get|post']);
```
后缀检测:
```php
Route::get('new/:id','News/read',['ext'=>'shtml|html']);
//禁止的后缀
// 定义GET请求路由规则 并设置禁止URL后缀为png、jpg和gif的访问
Route::get('new/:id','News/read',['deny_ext'=>'jpg|png|gif']);
```
域名检测:
```php
// 完整域名检测 只在news.thinkphp.cn访问时路由有效
Route::get('new/:id','News/read',['domain'=>'news.thinkphp.cn']);
// 子域名检测
Route::get('new/:id','News/read',['domain'=>'news']);
```
https检测:
```php
Route::get('new/:id','News/read',['https'=>true]);
```
缓存路由请求:
```php
Route::get('new/:name$','News/read',['cache'=>3600]);
```

8、可以稍微看一下资源路由和MISS路由<br>

9、如何进行域名部署路由功能?<br>
开启域名部署路由功能:
```php
'url_domain_deploy' =>  true
```
动态注册:
```php
// blog子域名绑定到blog模块
Route::domain('blog','blog');
// 完整域名绑定到admin模块
Route::domain('admin.thinkphp.cn','admin');
// IP绑定到admin模块
Route::domain('114.23.4.5','admin');
```
配置定义注册:
```php
return [
    '__domain__'=>[
        'blog'      => 'blog',
        // 泛域名规则建议在最后定义
        '*.user'    =>  'user',
        '*'         => 'book',
    ],
    // 下面是路由规则定义
]
```

10、如何进行url生成?<br>
如我的路由规则是这样:
```php
'test/:id' => ['index/index/test',['method' => 'get'],['id'=>'\d+']],
'news/:year/:month/:day' => ['Index/news',['method' => 'get'],['year'=>'\d{4}','month'=>'\d{2}','day'=>'\d{1,2}']],
'blog/:name$' => ['Index/blog',['method' => 'get']],
'item-<name>-<id>' => ['Index/detail',['method' => 'get'],['name'=>'[a-zA-Z]+','id'=>'\d+']],
'gds/:id' => ['Index/goods',['method' => 'get'],['ext'=>'shtml|html']],
```
则可以这样生成url:
```php
url('index/index/test',['id'=>5]); // /tp5/public/index/test/5.html
url('Index/detail', ['name' => 'bbg', 'id' => 123]);// /tp5/public/item-bbg-123.shtml
```
如何要指定域名,则自动生成域名如下:
```php
url('Index/detail', ['name' => 'bbg', 'id' => 123],'shtml',true);
//http://当前域名/tp5/public/item-bbg-123.shtml
```
手动指定域名如下:
```php
url('Index/detail', ['name' => 'bbg', 'id' => 123],'shtml','blog.thinkphp.cn');
//http://blog.thinkphp.cn/tp5/public/item-bbg-123.shtml
```
其他示例如下:
```php
//路由规则:
Route::rule('blog/:id','index/blog/read');
//生成url:
Url::build('index/blog/read','id=5&name=thinkphp');
Url::build('index/blog/read',['id'=>5,'name'=>'thinkphp']);
url('index/blog/read','id=5&name=thinkphp');
url('index/blog/read',['id'=>5,'name'=>'thinkphp']);
```
11、如何设置URL后缀?
```php
'url_html_suffix'   => 'shtml'
```

12、设置了'url_common_param'=>true有什么用?<br>
路由规则是这样:
```php
Route::rule('blog/:id','index/blog/read');
```
生成的url是这样:
```php
/index.php/blog/5.html?name=thinkphp
```
