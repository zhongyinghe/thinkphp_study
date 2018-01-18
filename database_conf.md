1、数据库配置<br>
1)应用基本配置(database.php):
```php
return [
    // 数据库类型
    'type'        => 'mysql',
    // 数据库连接DSN配置
    'dsn'         => '',
    // 服务器地址
    'hostname'    => '127.0.0.1',
    // 数据库名
    'database'    => 'thinkphp',
    // 数据库用户名
    'username'    => 'root',
    // 数据库密码
    'password'    => '',
    // 数据库连接端口
    'hostport'    => '',
    // 数据库连接参数
    'params'      => [],
    // 数据库编码默认采用utf8
    'charset'     => 'utf8',
    // 数据库表前缀
    'prefix'      => 'think_',
    // 数据库调试模式
    'debug'       => false,
    // 数据库部署方式:0 集中式(单一服务器),1 分布式(主从服务器)
    'deploy'      => 0,
    // 数据库读写是否分离 主从式有效
    'rw_separate' => false,
    // 读写分离后 主服务器数量
    'master_num'  => 1,
    // 指定从服务器序号
    'slave_no'    => '',
    // 是否严格检查字段是否存在
    'fields_strict'  => true,    
];
```
2)模块配置,在模块下新增database.php文件(基本类似基本配置)，添加如下:
```php
return [
    // 服务器地址
    'hostname'    => '192.168.1.100',
    // 数据库名
    'database'    => 'admin',    
];
```
3)方法配置:
```php
Db::connect([
    // 数据库类型
    'type'        => 'mysql',
    // 数据库连接DSN配置
    'dsn'         => '',
    // 服务器地址
    'hostname'    => '127.0.0.1',
    // 数据库名
    'database'    => 'thinkphp',
    // 数据库用户名
    'username'    => 'root',
    // 数据库密码
    'password'    => '',
    // 数据库连接端口
    'hostport'    => '',
    // 数据库连接参数
    'params'      => [],
    // 数据库编码默认采用utf8
    'charset'     => 'utf8',
    // 数据库表前缀
    'prefix'      => 'think_',
]);
```
或者这样:
```php
Db::connect('mysql://root:1234@127.0.0.1:3306/thinkphp#utf8');
```
如果在database.php文件中添加这样的配置信息:
```php
//数据库配置1
'db_config1' => [
    // 数据库类型
    'type'        => 'mysql',
    // 服务器地址
    'hostname'    => '127.0.0.1',
    // 数据库名
    'database'    => 'thinkphp',
    // 数据库用户名
    'username'    => 'root',
    // 数据库密码
    'password'    => '',
    // 数据库编码默认采用utf8
    'charset'     => 'utf8',
    // 数据库表前缀
    'prefix'      => 'think_',
],
//数据库配置2
'db_config2' => 'mysql://root:1234@localhost:3306/thinkphp#utf8';
```
则我们可以这样设置:
```php
Db::connect('db_config1');
Db::connect('db_config2');
```
4)模型配置:
```php
//在模型里单独设置数据库连接信息
namespace app\index\model;

use think\Model;

class User extends Model
{
    protected $connection = [
        // 数据库类型
        'type'        => 'mysql',
        // 数据库连接DSN配置
        'dsn'         => '',
        // 服务器地址
        'hostname'    => '127.0.0.1',
        // 数据库名
        'database'    => 'thinkphp',
        // 数据库用户名
        'username'    => 'root',
        // 数据库密码
        'password'    => '',
        // 数据库连接端口
        'hostport'    => '',
        // 数据库连接参数
        'params'      => [],
        // 数据库编码默认采用utf8
        'charset'     => 'utf8',
        // 数据库表前缀
        'prefix'      => 'think_',
    ];
}
```
或者这样:
```php
//在模型里单独设置数据库连接信息
namespace app\index\model;
use think\Model;
class User extends Model
{
    //或者使用字符串定义
    protected $connection = 'mysql://root:1234@127.0.0.1:3306/thinkphp#utf8';
}
```
或者这样:
```php
//在模型里单独设置数据库连接信息
namespace app\index\model;
use think\Model;
class User extends Model
{
    // 直接使用配置参数名
    protected $connection = 'db_config1';
}
```
5)主从配置:
```php
//分布式数据库配置定义
return [
    // 启用分布式数据库
    'deploy'    =>  1,
    // 数据库类型
    'type'        => 'mysql',
    // 服务器地址
    'hostname'    => '192.168.1.1,192.168.1.2',
    // 数据库名
    'database'    => 'demo,demo2',
    // 数据库用户名
    'username'    => 'root,root2',
    // 数据库密码
    'password'    => 'pwd1,pwd2',
    // 数据库连接端口
    'hostport'    => '3306,3306',
    //读写分离
    'rw_separate' => true,
]
```
