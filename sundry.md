1、thinkphp的缓存<br>
1)应用配置文件中配置:
```php
'cache'         => [
    // 驱动方式
    'type'   => 'File',
    // 缓存保存目录
    'path'   => CACHE_PATH,
    // 缓存前缀
    'prefix' => '',//如果设置了前缀，则文件缓存会以前缀建立目录
    // 缓存有效期 0表示永久缓存
    'expire' => 0,
],
```
2)使用缓存
```php
Cache::set('myname', 'sogua', 3600);//设置数据
$name = Cache::get('last_name');//缓存数据
dump($name);

Cache::rm('first_name');//删除缓存
Cache::clear();//清空所有的缓存
```
2、redis缓存<br>
1)在extend目录下建立redis目录，并新建Redis.php文件，把redis的操作写进代文件里
```php
<?php
class Redis {
}
```
2)在application下的extra目录下新建redis.php文件，作为配置
```php
return [
	'redis' => [
		'host' => '127.0.0.1',
		'port' => '6379',
		'auth' => '123456',
	],
	'redis_attr' => ['db_id' => 0],
];
```
3)在common.php文件下新建函数
```php
//获取redis对象
function getRedisObj() {
	$config = config('redis.redis');
	$attr   = config('redis.redis_attr');
	return \redis\Redis::getInstance($config, $attr);
}
```
3、session的配置(应用配置文件中)<br>
1)默认配置
```php
'session'      => [
	'id'             => '',
	// SESSION_ID的提交变量,解决flash上传跨域
	'var_session_id' => '',
	// SESSION 前缀
	'prefix'         => 'think',
	// 驱动方式 支持redis memcache memcached
	'type'           => '',
	// 是否自动开启 SESSION
	'auto_start'     => true,
],
```
2)把session保存在redis中
```php
'session'       => [
	'prefix'     => 'think',
	'type'       => 'redis',
	'auto_start' => true,
	// redis主机
	'host'       => '127.0.0.1',
	// redis端口
	'port'       => 6379,
	// 密码
	'password'   => '123456',
],
```
3)使用session函数
```php
// 赋值（当前作用域）
Session::set('name','thinkphp');
// 赋值think作用域
Session::set('name','thinkphp','think');
// 取值（当前作用域）
Session::get('name');
// 取值think作用域
Session::get('name','think');
// 删除（当前作用域）
Session::delete('name');
// 删除think作用域下面的值
Session::delete('name','think');
// 清除session（当前作用域）
Session::clear();
// 清除think作用域
Session::clear('think');
```
