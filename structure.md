1、什么是入口文件?
```
答:是各模块访问的入口。

	1)入口文件在哪里?
	答:在public/index.php
	
	2)入口文件主要是做什么的?
	答:运行webapp
```
2、如何让nginx支持PATHINFO模式?即http://serverName/index.php/模块/控制器/操作/[参数名/参数值...]可以这样访问thinkphp
```
1)关于个人原先无法使用nginx变成PATHINFO模式的原因是什么？
		答:在location ~ \.php$ {这里设置有问题，因为我这个是以.php结尾,所以报错；
		   正确做法是location ~ \.php($|/) {是这样。网站的配置参考是:http://www.thinkphp.cn/topic/40391.html
```
3、如何改命名空间名称?
```
答:在应用配置中进行类似这样的'app_namespace' => 'application',配置
```
4、是否支持多个模块的判断是?
```
答:'app_multi_module'  =>  true, // 开启/关闭多模块设计
```
5、使用第三方类库/PHP内置类库该怎么做?
```
答:在类的前面加\
```
6、thinkphp5.0有哪3个根命名空间?
```
答:think、traits和app
```
7、自己另外写的类放在哪里?(自动注册)
```
答:extend目录下,这样可以自动注册
注意:在使用时无需在前面使用use来加载;但在new对象时要在命名空间前加\
```
8、如何进行手动注册?(7和8可以对比看)
```
答:在common.php中这样注册:
\think\Loader::addNamespace([
    'my'  => '../application/extend/my/',
    'org' => '../application/extend/org/',
]);

或者在应用配置文件中这样注册:
'root_namespace' => [
    'my'  => '../application/extend/my/',
    'org' => '../application/extend/org/',
]
```
9、如何进行应用类库包书写?
```php
namespace app\index\model;

class User extends \think\Model
{
}
```
或者
```php
namespace app\index\model;

use think\Model;

class User extends Model
{
}
```
则它的使用:
```php
namespace app\index\controller;
use app\index\model\User;
class Index
{
    public function index()
    {
        $user = new User();
    }
}
```
10、如何进行类库导入?
```php
Loader::import('org.util.array');
Loader::import('@.util.upload');
```
11、如何确定返回类型?
```
答:'default_return_type'=>'json',也可以使用其他方式,支持类型有json,xml,jsonp
```
