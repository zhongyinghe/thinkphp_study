1、插入数据
```php
//插入形式一
$userinfo = new User();
$userinfo->username = 'mmc';
$userinfo->departname = '模特c';
$userinfo->created = time();
$userinfo->save();

//插入形式二
$user = new User;
$user->data([
    'username' => '金发碧眼',
    'departname' => '外国',
    'created' => time(),
]);

//插入形式三——排除非数据表字段
$data = [
    'username' => '天空之城',
    'departname' => '未来部',
    'created' => time(),
    'abc' => 'abc'
];

$user = new User;
$user->data($data)->allowField(true)->save();//abc非表字段，使用allowField(true)
//$user->data($data)->save();//这个会报错

//插入形式四——指定字段插入表
$data = [
    'username' => '天空之城',
    'departname' => '未来部',
    'created' => time(),
    'abc' => 'abc'
];

$user = new User;
$user->data($data)->allowField(['username','departname'])->save();//create是表字段，但没有指定

//插入多条记录
$user = new User;
$datas = [
    ['username' => '歌曲1', 'departname' => '音乐', 'created' => time(), 'abc' => 'fff'],
    ['username' => '歌曲2', 'departname' => '音乐', 'created' => time(), 'abc' => 'fff'],
    ['username' => '歌曲3', 'departname' => '音乐', 'created' => time(), 'abc' => 'fff'],
];

$user->allowField(true)->saveAll($datas);//abc非表字段
