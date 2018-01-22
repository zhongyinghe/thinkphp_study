1、子查询
```php
$rs = Db::table('userinfo')->where('uid', 'IN', function($query){
      $query->table('userinfo')->where('username', 'like', 'ab%')->field('uid');
})->select();
dump($rs);

$rs = Db::table('userinfo')->alias('u')->where(function($query){
      $query->table('userscore')->alias('us')->where('u.uid=us.uid')->field('us.uid');
},'exists')->select();
dump($rs);
echo Db::getLastSql();//SELECT * FROM `userinfo` `u` WHERE EXISTS ( SELECT `us`.`uid` FROM `userscore` `us` WHERE ( u.uid=us.uid ) )
```
