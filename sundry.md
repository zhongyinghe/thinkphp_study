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
4、cookie的安全设置
```php
'cookie'     => [
	// cookie 名称前缀
	'prefix'    => '',
	// cookie 保存时间
	'expire'    => 0,
	// cookie 保存路径
	'path'      => '/',
	// cookie 有效域名
	'domain'    => '',
	//  cookie 启用安全传输
	'secure'    => false,
	// httponly设置
	'httponly'  => true,//这个地方设为true,然后再加上uuid验证,则可以防止xss攻击
	// 是否使用 setcookie
	'setcookie' => true,
],
```
5、多语言设置<br>
在application下新建lang目录,然后新增zh-cn.php、en-us.php文件，文件如下:
```php
<?php 
//zh-cn.php文件
return [
	'user_name' => '用户名',
];
```
```php
<?php 
//en-us.php文件
return [
	'user_name' => 'UserName',
];
```
在应用配置上进行如下配置:
```php
// 是否开启多语言
'lang_switch_on'         => true,
// 默认语言
'default_lang'           => 'zh-cn',
```
注意事项:
```
'default_lang' => 'en-us'
是在
'lang_switch_on' => false,
才会生效的，
开启了多语言，就会变成自动侦测
１.　是否有$_GET['lang']
２.　识别$_SERVER['HTTP_ACCEPT_LANGUAGE']中的第一个语言
因为大家在国内，所以侦测到的都是zh-cn
```
6、分页<br>
php代码:
```php
$list = Db::table('userinfo')->paginate(10);
$this->assign([
	'list' => $list,
]);
return $this->fetch();
```
html代码:
```html
<div>
<ul>
<volist name='list' id='user'>
    <li><{$user.username}></li>
</volist>
</ul>
</div>
<{$list->render()}>
```
7、文件上传<br>
1)模板
```html
<form action="<{:url('index/sundry/upload')}>" enctype="multipart/form-data" method="post">
<input type="file" name="image" /> <br> 
<input type="submit" value="上传" /> 
</form> 
```
```html
<form action="<{:url('index/sundry/upload')}>" enctype="multipart/form-data" method="post">
<input type="file" name="image[]" /> <br> 
<input type="file" name="image[]" /> <br>
<input type="file" name="image[]" /> <br> 
<input type="submit" value="上传" /> 
</form> 
```
2)处理程序
```php
if (Request::instance()->isGet()) {
	return $this->fetch();
}else {
	$file = Request::instance()->file('image');
	if ($file) {
	$info = $file->validate(['size' => 1024*1024, 'ext' => 'jpg,png,gif'])->rule('myfilename')->move(ROOT_PATH . 'public' . DS . 'upload');
		if ($info) {
			echo $info->getExtension();
			echo '<br>';
			echo $info->getSaveName();
			echo '<br>';
			echo $info->getFileName();
		} else {
			// 上传失败获取错误信息
			echo $file->getError();
		}
	}
}
```
```php
$files = Request::instance()->file('image');
foreach($files as $file) {
	$info = $file->move(ROOT_PATH . 'public' . DS . 'upload');
	if($info) {
	}else {
		echo $file->getError();
	}
}
```
3)验证上传文件
```php
$info = $file->validate(['size' => 1024*1024, 'ext' => 'jpg,png,gif'])->move(ROOT_PATH . 'public' . DS . 'upload');
```
4)文件生成规则
```php
//默认的有md5、date、sha1
$info = $file->rule('md5')->move(ROOT_PATH . 'public' . DS . 'upload');
//可以自定义规则,然后在common.php文件中定义myfilename函数就可以了
$info = $file->rule('myfilename')->move(ROOT_PATH . 'public' . DS . 'upload');
```
