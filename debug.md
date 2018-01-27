1、日志<br>
1)配置信息:
```php
'log'                    => [
  // 日志记录方式，内置 file socket 支持扩展
  'type'  => 'File',
  // 日志保存目录
  'path'  => LOG_PATH,
  // 日志记录级别
  'level' => [],
  //独立日志，在独立的文件记录该等级的日志
  'apart_level'   =>  ['error','sql'],
],
```
2)日志写入方法:
```php
Log::record();//把信息记录到内存
Log::save();//把信息写入文件
Log::write();//实时写入一条日志信息

//快速记录方法
Log::info();
Log::error();//等等...
```
3)日志等级
```
log 常规日志，用于记录日志
error 错误，一般会导致程序的终止
notice 警告，程序可以运行但是还不够完美的错误
info 信息，程序输出信息
debug 调试，用于调试信息
sql SQL语句，用于SQL记录，只在数据库的调试模式开启时有效
```
2、错误和调试<br>
1)调试模式.开启调试模式,在应用配置文件中
```php
/ 关闭调试模式
'app_debug' => true,
```
2)抛出异常
```php
//一般异常
throw new \think\Exception('主动异常', 100000096);
//http异常
throw new \think\exception\HttpException(404, '我抛出异常消息', null, []);
```
3)trace调试.在应用配置中
```php
// 应用Trace
'app_trace'              => true,
//trace设置
'trace'                  => [
  // 内置Html Console 支持扩展
  'type' => 'Html',
],
```
4)性能调试
```php
Debug::remark('begin');
//$rs = [];
Db::table('userinfo')->chunk(3, function($users){
  foreach($users as $user) {
    dump($user);
  }
  echo '--------------------------';
});

$rs = Db::table('userinfo')->select();
dump($rs);
Debug::remark('begin');
echo '<br>';
echo Debug::getRangeTime('begin', 'end').'s';//计算执行区间花费的时间
echo '<br>';
echo Debug::getRangeMem('begin', 'end').'kb';//计算执行区间需要的内存,要环境支持内存统计才行
```
5)SQL调试<br>
在database.php配置文件中进行如下配置:
```php
// 数据库调试模式
'debug'           => true,
// 是否需要进行SQL性能分析
'sql_explain'     => false,
```
这样会在日志文件中对sql语句进行:EXPLAIN sql<br>

6)404错误.在应用配置文件中进行配置
```php
'http_exception_template'    =>  [
    // 定义404错误的重定向页面地址
    404 =>  APP_PATH.'404.html',
    // 还可以定义其它的HTTP status
    401 =>  APP_PATH.'401.html',
]
```
