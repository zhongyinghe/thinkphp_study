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
2、事务支持
```php
Db::startTrans();	
try{
      /*$dxs = Db::table('userscore')->where('uid', 19)->find();
      $gxss = Db::table('userscore')->where('uid', 15)->find();
      $dxr = $dxs['score'] - 5;
      $gxxr = $gxss['score'] + 5;
      Db::table('userscore')->where('uid', 19)->update(['score' => $dxr]);
      Db::table('userscore')->where('uid', 15)->update(['score' => $gxxr]);*/
      Db::table('userscore')->where('uid', 19)->setDec('score', 5);
      Db::table('userscore')->where('uid', 15)->setInc('score', 5);
      Db::commit();
}catch(\Exception $e) {
      Db::rollback();
}
```
