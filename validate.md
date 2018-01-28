1、函数内部使用验证<br>
1)基本使用
```php
//规则
$rule = [
  'name' => ['require', 'max' => 25],
  'email' => 'email',
  'age' => ['number', 'between' => '1,120'],
];
//错误信息
$msg = [
  'name.require' => '必须有名称',
  'name.max' => '名称最多不能超过25个字符',
  'age.number' => '年龄必须为数字',
  'age.between' => '年龄在1-120之间',
  'email' => '邮件格式错误',
];
//使用
$validate = new Validate($rule, $msg);
//验证数据
$data = [
  'name' => 'thinkphp',
  'email' => 'thinkphp@qq.com',
  'age' => 141,
];

if (!$validate->check($data)) {
  dump($validate->getError());
}
```
2)动态扩展验证规则
```php
$validate->rule('zip', '/^\d{6}/');
```
3)增加验证函数
```php
$validate = new Validate(['name' => 'checkName:1']);
$validate->extend([
    'checkName'=> function ($value, $rule) {
    return $rule == $value ? true : '名称错误';
},
    'checkStatus'=> [$this,'checkStatus']
]);
```
4)验证场景
```php
$validate = new Validate($rule);

$validate->scene('edit', ['name', 'age']);

if (!$validate->scene('edit')->check($data)) {
  dump($validate->getError());
}
```

2、验证类
```php
<?php 
namespace app\index\validate;

use think\Validate;

class User extends Validate {
    protected $rule = [
        'name' => 'require|max:25',
        'age' => 'number|between:1,120',
        'email' => 'email',
        'password' => 'checkPwd:/^\d{6}$/'
    ];

    protected $message = [
        'name.require' => '必须有名称',
        'name.max' => '名称最多不能超过25个字符',
        'age.number' => '年龄必须为数字',
        'age.between' => '年龄在1-120之间',
        'email' => '邮件格式错误',
        'password' => '密码必须为6位数字',
    ];

    protected $scene = [
        'edit' => ['name', 'age' => 'between:1,160', 'password'],
        'add' => ['name', 'age', 'email', 'password'],
    ];

    protected function checkPwd($value, $rule) {
        return preg_match($rule, $value) > 0 ? true : $this->message['password'];
    }
}
```
使用方法:
```php
$data = [
    'name' => 'thinkphp',
    'email' => 'thinkphp@qq.com',
    'age' => 150,
    'password' => '12345678910',
];

$validate = Loader::validate('User');

if (!$validate->scene('edit')->check($data)) {
    dump($validate->getError());
}
```

3、静态调用
```php
$rs = Validate::regex('abc123', '\d+');
dump($rs);
```
