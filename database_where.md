1、where方法,相当于AND
```php
$row = Db::table('userinfo')->where('username', 'like','%yi%')->where('uid',2)->select();
```
2、whereOr方法，相当于OR
```php
$rows = Db::table('userinfo')->where('username', 'like','%yi%')
		->whereOr('departname','like', '%部')->select();
```
3、混合操作
```php
$rows = Db::table('userinfo')->where(function($query){
		 	$query->where('username', 'like', '%yi%');
		})->whereOr(function($query){
		 	$query->where('departname', 'like', '%部');
		})->select();
```
4、获取表格信息
```php
$info = Db::getTableInfo('userinfo');
dump($info); 

$fields = Db::getTableInfo('userinfo', 'fields');
dump($fields);

$type = Db::getTableInfo('userinfo', 'type');
dump($type);

$pk = Db::getTableInfo('userinfo', 'pk');
dump($pk);
```
5、查询语法<br>
eq:
```php
where('id','eq',100);
where('id','=',100);
where('id',100);
```
neq:
```php
where('id','neq',100);
where('id','<>',100);
```
gt:
```php
where('id','gt',100);
where('id','>',100);
```
egt:
```php
where('id','egt',100);
where('id','>=',100);
```
lt:
```php
where('id','lt',100);
where('id','<',100);
```
elt:
```php
where('id','elt',100);
where('id','<=',100);
```
[not]like:
```php
where('name','like','thinkphp%');
where('name','like',['%think','php%'],'OR');
```
[not]between:
```php
where('id','between','1,8');
where('id','not in',[1,5,8]);
```
[not]null:
```php
where('name', null);
where('title','null');
where('name','not null');
where('title','=', 'null');
where('name','=', 'not null');
```
exp表达式:
```php
where('id','in','1,3,8');
```
改成这样:
```php
where('id','exp',' IN (1,3,8) ');
```
6、where查询形式<br>
1)表达式条件:
```php
$row = Db::table('userinfo')->where('username', 'like','%yi%')->where('uid',2)->select();
```
2)数组条件
```php
$map['username'] = 'liuyifei';
$map['departname'] = '演艺部';
$rows = Db::table('userinfo')->where($map)->select();
dump($rows);

$map = [];
$map['uid'] = ['>', 1];
$map['username'] = ['like', 'ab%'];
$rows = Db::table('userinfo')->where($map)->select();
dump($rows);
```
3)字符串条件
```php
$rows = Db::table('userinfo')->where("uid = 12 AND departname = 'AI'")->select();
```
7、高级查询<br>
1)区间查询:
```php
$rs = Db::table('userinfo')->where('uid',['>', 6], ['<', 16])->select();
dump($rs);
```
2)多条件查询:
```php
$rs = Db::table('userinfo')->where([
	'uid' => [['>', 6],['<', 16]],
	'username' => ['like', 'ab%'],
	])->select();
dump($rs);
```
3)闭包查询:
```php
$rs = Db::select(function($query){
	$query->table('userinfo')->where([
	'uid' => [['>', 6],['<', 16]],
	'username' => ['like', 'ab%'],
	]);
});
dump($rs);
```
4)query查询:
```php
$query = new \think\db\Query();
$query->table('userinfo')->where([
	'uid' => [['>', 6],['<', 16]],
	'username' => ['like', 'ab%'],
]);
$rs = Db::select($query);
dump($rs);
```
