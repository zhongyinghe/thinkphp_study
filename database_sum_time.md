1、聚合查询
```php
//计算总数
$count = Db::table('userinfo')->count();
dump($count);
//获取最大值
$max = Db::table('userinfo')->where('uid', '>', 5)->max('uid');
dump($max);
//获取最小值
$min = Db::table('userinfo')->where('uid', '>', 5)->min('uid');
dump($min);
//获取平均值
$avg = Db::table('userinfo')->where('uid', '>', 5)->avg('uid');
dump($avg);
//计算xx之和
$sum = Db::table('userinfo')->where('uid', '>', 5)->sum('uid');
dump($sum);
```

2、时间查询函数
```php
// 大于某个时间
where('create_time','> time','2016-1-1');
// 小于某个时间
where('create_time','<= time','2016-1-1');
// 时间区间查询
where('create_time','between time',['2015-1-1','2016-1-1']);

// 大于某个时间
Db::table('think_user')->whereTime('birthday', '>=', '1970-10-1')->select();
// 小于某个时间
Db::table('think_user')->whereTime('birthday', '<', '2000-10-1')->select();
// 时间区间查询
Db::table('think_user')->whereTime('birthday', 'between', ['1970-10-1', '2000-10-1'])->select();
// 不在某个时间区间
Db::table('think_user')->whereTime('birthday', 'not between', ['1970-10-1', '2000-10-1'])->select();

// 获取今天的博客
Db::table('think_blog') ->whereTime('create_time', 'today')->select();
// 获取昨天的博客
Db::table('think_blog')->whereTime('create_time', 'yesterday')->select();
// 获取本周的博客
Db::table('think_blog')->whereTime('create_time', 'week')->select();   
// 获取上周的博客
Db::table('think_blog')->whereTime('create_time', 'last week')->select();    
// 获取本月的博客
Db::table('think_blog')->whereTime('create_time', 'month')->select();   
// 获取上月的博客
Db::table('think_blog')->whereTime('create_time', 'last month')->select();      
// 获取今年的博客
Db::table('think_blog')->whereTime('create_time', 'year')->select();    
// 获取去年的博客
Db::table('think_blog')->whereTime('create_time', 'last year')->select();     


// 获取今天的博客
Db::table('think_blog')->whereTime('create_time', 'd')->select();
// 获取本周的博客
Db::table('think_blog')->whereTime('create_time', 'w')->select();   
// 获取本月的博客
Db::table('think_blog')->whereTime('create_time', 'm')->select();   
// 获取今年的博客
Db::table('think_blog')->whereTime('create_time', 'y') ->select();    


// 查询两个小时内的博客
Db::table('think_blog')->whereTime('create_time','-2 hours')->select();
```
