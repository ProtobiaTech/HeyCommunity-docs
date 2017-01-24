# 构建与部署

目前 HeyCommunity 还处于快速开发迭代阶段，安装介绍可能不够详细，遇到问题请[与我们联系](../other/contact.html)。   


## 获取完整的项目代码

```
$ git clone https://github.com/dev4living/HeyCommunity.git HeyCommunity
$ cd HeyCommunity
$ cd HeyCommunity
$ git submodule update --init --recursive --depth 1         ## This step may be slow, and you can get the source code from Releases
```


## 前端配置

前端基于 [Ionic Framework](http://ionicframework.com)，遇到问题请查阅Ionic官方文档。

```
$ cd HeyCommunity/frontend
$ npm install
$ npm run ionic:build
$ npm install ionic cordova                     ## Optional, install the ionic cordova to build the app
$ ionic build ios                               ## Optional, use the ionic build ios app
$ open platforms/ios/HeyCommunity.xcodeproj     ## Optional, use xcode to open HeyCommunity.xcodeproj, the simulator runs, real machine test, upload to the AppStore
```


## 后端配置

前端基于[Laravel Framework](http://laravel.com)，遇到问题请查阅Laravl官方文档。

```
$ cd HeyCommunity/backend
$ composer install
$ bower install
$ cp .env.example .env
$ php artisan key:generate
$ vi .env                                       ##  Configure the database connection
# php artisan migrate:refresh --seed            ##  Build the database and generate fake data
```


## 运行

我们目前仅支持 Apache 服务器，假设已经把 `demo.hey-community.local` 解析到了 `HeyCommunit目录`   
那么在浏览器中打开 `demo.hey-community.local` 即可体验和预览 HeyCommunity 在浏览器中的运行   

IOS 和 Android 设备上的运行暂不做介绍，如有需要请查看Ionic官方文档。
