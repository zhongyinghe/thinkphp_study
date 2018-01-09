1、配置文件怎么分类?<br>
答:分为应用配置和模块配置<br>

2、应用配置和模块配置分别放在哪里?<br>
答:应用配置在application下的config.php;而模块配置在模块下的config.php.<br>

3、如何改变配置的目录?
答:在入口文件重新定义CONF_PATH的常量.
```php
// 定义配置文件目录和应用目录同级
define('CONF_PATH', __DIR__.'/../config/');
```

4、如何增加扩展目录?<br>
答:在应用目录下或者在模块目录下新增extra目录.然后新建文件，通过return []来进行扩展配置


5、配置格式有哪些?<br>
答:PHP、ini、xml、json

6、请列出示例<br>
答:
```
//项目配置文件
return [
    // 默认模块名
    'default_module'        => 'index',
    // 默认控制器名
    'default_controller'    => 'Index',
    // 默认操作名
    'default_action'        => 'index',
    //更多配置参数
    //...
];
```
7、二级配置是如何进行配置的?
答:
php的
```
//项目配置文件
return [
    'cache'                 => [
        'type'   => 'File',
        'path'   => CACHE_PATH,
        'prefix' => '',
        'expire' => 0,
    ],
];
```
ini的
```
[user]
type=1
name=thinkphp
 [db]
type=mysql
user=rot
password=''
```

8、如何修改配置文件的后缀?
答:
```
//在入口文件中更改配置格式为ini格式
define('CONF_EXT', '.ini');
```
