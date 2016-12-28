# 自主安装

目前 HeyCommunity 还处于快速开发迭代阶段，安装介绍可能不够详细，遇到问题请[与我们联系](../contact/readme.html)。   


## 获取完整的项目代码

```
$ git clone https://github.com/dev4living/HeyCommunity.git HeyCommunity
$ cd HeyCommunity
$ git submodule init
$ git submodule update
```


## 前端配置

前端基于 [Ionic Framework](http://ionicframework.com)，遇到问题请查阅Ionic官方文档。

```
$ cd HeyCommunity/frontend
$ bower install
$ npm install
$ vi www/js/config.js           ## 可能需要修改些配置
```


## 后端配置

前端基于[Laravel Framework](http://laravel.com)，遇到问题请查阅Laravl官方文档。

```
$ cd HeyCommunity/backend
$ composer install
$ bower install
$ cp .env.example .env
$ php artisan key:generate
$ vi .env                       ## 配置数据库连接
$ php artisan migrate:refresh --seed
```


## 运行

我们目前仅支持 Apache 服务器，假设已经把 `demo.hey-community.local` 解析到了 `HeyCommunit目录`   
那么在浏览器中打开 `demo.hey-community.local` 即可体验和预览 HeyCommunity 在浏览器中的运行   

IOS 和 Android 设备上的运行暂不做介绍，如有需要请查看Ionic官方文档。


