1、别名使用
```php
$rs = Db::table('userinfo')->alias('u')->join('userscore s', 'u.uid = s.uid')->select();
```
2、field的使用<br>
1)指定字段:
```php
$rs = Db::table('userinfo')->field('uid, username, departname')->select();
```
2)使用数组参数:
```php
$rs = Db::table('userinfo')->field(['uid', 'username', 'departname'])->select();
$rs = Db::table('userinfo')->field(['uid', 'concat(username,"-",uid)' => 'truename', 'departname'])->select();
```
3)排除字段:
```php
$rs = Db::table('userinfo')->field('departname, created', true)->select();
```
4)字段检测:
```php
$data = ['username' => 'zzx', 'departname' => '演艺部', 'created' => time()/*, 'password' => '123456'*/];
$rs = Db::table('userinfo')->field('username, departname, created')->insert($data);
```
3、order和limit
```php
$rs = Db::table('userinfo')->where('uid', '>', 2)->order('uid DESC')->limit(5)->select();
dump($rs);

$rs = Db::table('userinfo')->where('uid', '>', 2)->order('uid DESC')->limit(5,5)->select();
dump($rs);
```
page示例:
```php
$rs = Db::table('userinfo')->where('uid', '>', 2)->order('uid DESC')->page(1,5)->select();
$rs = Db::table('userinfo')->where('uid', '>', 2)->order('uid DESC')->page(2,5)->select();
```
4、group by和having<br>
1)group by:
```php
Db::table('think_user')
    ->field('user_id,username,max(score)')
    ->group('user_id')
    ->select();
```
生成的sql:
```php
SELECT user_id,username,max(score) FROM think_score GROUP BY user_id
```
2)having:
```php
Db::table('think_user')
    ->field('username,max(score)')
    ->group('user_id')
    ->having('count(test_time)>3')
    ->select();
```
生成的sql:
```php
SELECT username,max(score) FROM think_score GROUP BY user_id HAVING count(test_time)>3
```

5、连接操作<br>
### INNER JOIN:
等同于 JOIN（默认的JOIN类型）,如果表中有至少一个匹配，则返回行
```php
$rs = Db::table('userinfo')->alias('u')->join('userscore s', 'u.uid = s.uid')->select();
```
### LEFT JOIN:
即使右表中没有匹配，也从左表返回所有的行
```php
$rs = Db::table('userinfo')->alias('u')->join('userscore s', 'u.uid = s.uid', 'LEFT')->select();
```
### RIGHT JOIN:
 即使左表中没有匹配，也从右表返回所有的行
 ```php
 $rs = Db::table('userinfo')->alias('u')->join('userscore s', 'u.uid = s.uid', 'RIGHT')->select();
 ```
