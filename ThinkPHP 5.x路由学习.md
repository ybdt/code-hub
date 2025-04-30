# 路由模式
ThinkPHP5中的路由模式分为3种：默认模式、混合模式、强制模式

默认模式（PATH_INFO模式）：
```
// 是否开启路由
'url_route_on'  =>  false

形如：
http://127.0.0.1/index.php/module/controller/action/param1/value1/param2/value2/
对这块不了解的同学可以简单理解为，module是一个文件夹、controller是一个php文件、action是其中的一个方法、param和value是方法对应的参数名和参数值
```
强制模式
```
// 是否开启路由
'url_route_on'  		=>  true,
// 是否强制使用路由
'url_route_must'		=>  true,

此种模式下，必须为每一个访问地址定义路由规则，形如：
Route::get('/',function(){
    return 'Hello,world!';
});
是为首页定义路由规则
```
混合模式
```
// 是否开启路由
'url_route_on'           => true,
// 是否强制使用路由
'url_route_must'         => false,

此种模式下，只对需要定义路由规则的URL定义路由规则，其他URL仍然按照默认模式访问
```
# 注册路由

可以通过方法动态注册，也可以直接定义在路由定义文件中

## 动态注册

动态注册通常在应用的路由配置文件application/route.php中注册

```
use think\Route;
// 注册路由到index模块的News控制器的read操作
Route::rule('new/:id','index/News/read');
访问 http://serverName/new/5 自动路由到 http://serverName/index/news/read/id/5

审计thinkphp代码时，经常看到如下格式的路由
Route::get('new/:id','News/read'); // 定义GET请求路由规则
Route::post('new/:id','News/update'); // 定义POST请求路由规则
Route::put('new/:id','News/update'); // 定义PUT请求路由规则
Route::delete('new/:id','News/delete'); // 定义DELETE请求路由规则
Route::any('new/:id','News/read'); // 所有请求都支持的路由规则
上述是对应请求方法的简写

动态注册路由，完整格式如下
Route::rule("路由表达式", "路由地址", "请求方法", "匹配参数", "变量规则")

Route::rule([
    'new/:id'  =>  'News/read',
    'blog/:id' =>  ['Blog/update',['ext'=>'shtml'],['id'=>'\d{4}']],
    ...
],'','GET',['ext'=>'html'],['id'=>'\d+']);
以上的路由注册，最终 blog/:id 只会在匹配shtml后缀的访问请求，id变量的规则则是 \d{4}
```



## 路由定义文件

路由定义文件通常在应用下的route.php中，最后通过返回数组的方式直接定义路由规则，形如

```
return [
    'new/:id'   => 'News/read',
    'blog/:id'   => ['Blog/update',['method' => 'post|put'], ['id' => '\d+']],
];
路由配置定义中，效果和使用any注册动态路由是一致的

需要注意，路由动态注册和路由静态配置可以共存
```

# 其他部分

官方文档中的其他部分，有的是针对上述某一块内容的详细解释，有的一般用不上，都可以等用到时再看

# 参考文章

```
https://www.kancloud.cn/manual/thinkphp5/118029
```