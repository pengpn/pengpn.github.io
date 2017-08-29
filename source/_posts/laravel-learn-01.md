---
title: laravel基础学习记录笔记
date: 2017-08-29 23:40:54
tags: [laravel]
---
# laravel 学习笔记
## 常用函数：
- compact(‘变量1’,’变量2’)

类似于es6 的简写，会生成类似于['name' => $name]的键值对
- dd(变量)  

程序运行到此处终止并输出变量的值
<!--more--> 

## 常见问题
- 删除文件
删除完migration文件之后都要清除Composer中自动加载的记录，刷新自动加载记录
```
$ composer dump-autoload
```
## 技巧提点
- 进一步封装查询语句

Model.php
```
public function scopeIncompleted($query, $val)
{
    return $query->where('completed', 0)->get();
    //将查询语句进行封装，调用时相当于静态方法
    //Model::incompleted();
}
```

- Model与路由的绑定
```
Route::get('/tasks/{thetask}', 'TasksController@show');

    public function show(Task $thetask)
    {
        return view('tasks.show', compact('thetask'));
    }

//路由中括号内的参数名必须与Controller中的接收参数的变量名字一致，类型是Model对象
```
- 命名规则
controller => PostsController
Model => Post
migration => create_posts_table
- RESTFul
```
$ php artisan make:Controller TasksController -r
创建一个resourceful controller
```
- 文件路径更改
与其相关的命名空间，包括文件本身的命名空间都需要进行修改！

- 快速查看所有的路由
```
$ php artisan route:list
```
- 重定向返回至上一次请求尝试访问的页面
```
return redirect()->intended(route('users.show', [Auth::user()]));
```


## 关于Blade

> 声明title变量及其默认值
>
>
> @yield('content') // content区域
>
> @extends('layouts.default') @section('content') // 内容将会被插入布局模板中 @stop
> @yield 用于定义
>
>
> @section 用于插入值
>
>
> @include('shared.user_info', ['user' => $user])
>
>
> 给局部视图传参数
>
>
> old('name名称')
>
>
> 辅助函数显示旧的输入数据
>
## Model

 每一个Model内部都有三个属性


`$table`  `$fillable` `$hidden`

 指明交互的数据表名称，能进行更新的字段，JSON形式显示时隐藏的字段

## Controller

- 每一个controller中都有一个validator，可以用来验证字段

## Session 会话

- 创建会话实例


session()->flash('success', '欢迎，您将在这里开启一段新的旅程~');


flash方法使得会话缓存只在下一次请求中有效，第一个参数是键名，第二个参数是键值



- 访问会话实例


session()->get('success')

## Auth 认证系统
Auth::attempt()尝试登陆


Auth::user()当前登录实例


Auth::check() 是否已经登录


API:


```
bool attempt(array $credentials = array(), bool $remember = false)
```
第二个参数用来"记住我"

## 中间件的使用
- 在用户控制器构造方法中应用中间件

```
public function __construct()
    {
        $this->middleware('auth', [            
            'only' => ['edit', 'update']
        ]);
    }
```
- Authenticate.php中进行重定向
```
public function handle($request, Closure $next)
    {
        if ($this->auth->guest()) {
            if ($request->ajax()) {
                return response('Unauthorized.', 401);
            } else {
                return redirect()->guest('login');
            }
        }

        return $next($request);
    }
```

## 授权策略
已登录的用户之间的权限控制

- 创建Policy文件并创建用户比对方法

 ```
    public function update(User $currentUser, User $user)
    {
        return $currentUser->id === $user->id;
    }
    ```

- 在AuthServiceProvider中进行授权策略的配置，将用户模型指定授权策略
  ```
    protected $policies = [
        'App\Model' => 'App\Policies\ModelPolicy',
        User::class => UserPolicy::class
        //全路径匹配
    ];
    ```

- 将查询时返回的User实例发送给授权策略进行比较
  ```
   $user = User::findOrFail($id);
   $this->authorize('update', $user);
```
