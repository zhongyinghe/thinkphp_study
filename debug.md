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
