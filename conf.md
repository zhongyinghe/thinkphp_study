1、配置文件怎么分类?<br>
答:分为应用配置和模块配置<br>

2、应用配置和模块配置分别放在哪里?<br>
答:应用配置在application下的config.php;而模块配置在模块下的config.php.<br>

3、如何改变配置的目录?<br>
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
7、二级配置是如何进行配置的?<br>
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

8、如何修改配置文件的后缀?<br>
```
//在入口文件中更改配置格式为ini格式
define('CONF_EXT', '.ini');
```

9、配置文件的加载顺序是?<br>
答:惯例配置->应用配置->扩展配置->场景配置->模块配置->动态配置

10、惯例配置文件在哪里?<br>
答:`thinkphp/convention.php`

11、如何加载配置文件?<br>
```
Config::load(APP_PATH.'config/config.php');
```

12、如何读配置文件中的数据?
```php
echo Config::get('配置参数1');
```
或者
```php
Config::has('配置参数2');//判断配置参数是否存在
```
读二级的是这样:
```php
echo Config::get('配置参数.二级参数');
echo config('配置参数.二级参数');
```

13、设置配置参数的方法是?
```php
Config::set('配置参数','配置值');
// 或者使用助手函数
config('配置参数','配置值');

//批量设置
Config::set([
    '配置参数1'=>'配置值',
    '配置参数2'=>'配置值'
]);
// 或者使用助手函数
config([
    '配置参数1'=>'配置值',
    '配置参数2'=>'配置值'
]);
```
