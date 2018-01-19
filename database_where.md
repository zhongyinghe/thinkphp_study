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
