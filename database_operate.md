1、数据库的基本操作
```php
//查询操作
Db::query('SELECT * FROM userinfo WHERE uid = ?', [8]);
//更新、插入和删除操作
$affect = Db::execute('INSERT INTO userinfo (username, departname, created) VALUES(?, ?, ?)', ['tp5', 'PHP程序部', time()]);
$affect = Db::execute('INSERT INTO userinfo (username, departname, created) 
			VALUES(:username, :departname, :created)', ['username'=>'tp5', 'departname'=>'PHP程序部', 'created'=>time()]);
```
2、查询构造器——查询<br>
1)查询一行记录:
```php
$row = Db::table('userinfo')->where('uid', 10)->find();
```
2)查询多行记录:
```php
$rows = Db::table('userinfo')->where('uid', '>', 10)->select();
```
3)查询器查询:
```php
$query = new \think\db\Query();
$query->table('userinfo')->where('uid', 10);
$row = Db::find($query);
```
4)闭包查询:
```php
$rows = Db::select(function($query){
		$query->table('userinfo')->where('uid', '>', 10);
});
```
5)查询某一直:
```php
$value = Db::table('userinfo')->where('uid', 5)->value('username');//返回一个字符串
```
6)查询某一列:
```php
$columns = Db::table('userinfo')->where('uid','>', 5)->column('username, departname');//返回数组
```
7)分批查询:
```php
Db::table('userinfo')->where('uid', '>', 2)->chunk(3, function($users){
		foreach ($users as $user) {
			dump($user);
		}
		echo '====================';
});
```
