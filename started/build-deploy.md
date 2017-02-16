# 构建与部署

目前 HeyCommunity 还处于快速开发迭代阶段，安装介绍可能不够详细，遇到问题请[与我们联系](../other/contact.html)   
一些常见的问题，你也可以查看 [FAQ](/support/faq.md)


## 获取完整的项目代码

HeyCommunity 遵循 GPLv3 开源协议，源代码托管在 https://github.com/dev4living/HeyCommunity

```
$ git clone https://github.com/dev4living/HeyCommunity.git HeyCommunity
$ cd HeyCommunity
$ git submodule update --init --recursive               ## 这个步骤可能会比较缓慢，你也可以从 Github 下载最新的稳定源代码包
```


## 后端部署

后端基于 [Laravel Framework](http://laravel.com)，遇到问题请查阅 Laravl 官方文档   
你也可以**跳过这一步**，使用我们提供的 API，具体内容参见【前端构建】

后端部署依赖: 
- [Composer](http://www.phpcomposer.com)
- [Bower](https://bower.io)
- php >= 5.5.9
- MySQL
- Apache (需要开启 Rewrite 用于目录重写；设置 AllowOverride All 用于使 .httaccess 文件生效)
- FFmpeg （可选，用于获取视频的 Poster）

```
$ cd HeyCommunity/backend
$ composer install
$ bower install
$ cp .env.example .env
$ php artisan key:generate
$ vi .env                                       ##  进行数据库、微信公众号、文件系统等配置
# php artisan migrate:refresh --seed            ##  创建数据库并生成模拟数据
```

后端构建好之后你可以通过 `php artisan serve` 或 Apache 把后端部署起来为前端提供 API 。Apache 部署参见【运行】   
你也可以**跳过这一步**，使用我们提供的 API，具体内容参见【前端构建】



## 前端构建

前端基于 [Ionic Framework](http://ionicframework.com)，遇到问题请查阅 Ionic 官方文档

前端构建依赖:
- Node >= 6
- NPM
- Ionic 2.2
- Cordova 6.5


### 配置

你可以**跳过这个步骤**，使用我们已经在配置文件中提供的配置参数   
前端涉及的到配置文件有:

#### HeyCommunity/frontend/src/index.html
`index.html` 中的 `window.API_DOMAIN` 定义了 API URL，如果为空的话 API URL 默认为当前 URL   
如果 API URL 和当前 URL 不相同的话，会遇到 CORS 问题，你可以设置浏览器禁用跨域限制（推荐），或者使用 `ionic serve` 代理 API 请求   

#### HeyCommunity/frontend/ionic.config.json
`ionic.config.json` 中的 `proxies` 配置代理服务，`path` 定义需要代理的目录，`proxyUrl` 定义提供代理的 API URL（即真实的 API URL，通过部署后端获得）   

#### HeyCommunity/frontend/config.xml
`config.xml` 定义 Hybrid App 相关配置，具体内容请参见【构建 Hybrid App】

##### 解决问题 CORS 问题
先设置 `window.API_DOMAIN = null;`，再修改 `proxies` 配置正确的代理服务，最后使用 `ionic serve` 去启动 Web App   
`ionic serve` 会启动一个 Web Server，默认地址为 `htpp://localhost:8100`。现在通过这个地址打开 Web App 就不会出现 CORS 问题

最简单的方法是禁用浏览器跨域限制

##### 使用 HeyCommunity 提供的 API
我们已经在相关配置文件中使用了 HeyCommunity 提供的 API，只要你掌握了 `window.API_DOMAIN` 和 `proxies` 的用法，你就可以灵活运用它

#### 注意
Hybrid App 不会存在 CORS 问题，只需要通过 `window.API_DOMAIN` 定义完整的 API


### 构建 Web App
```
$ cd HeyCommunity/frontend
$ npm install
$ npm run ionic:build                           ##  构建 Web App
```

成功构建 Web App 之后，编译生成的文件会覆盖在 `HeyCommunity/frontend/www` 目录中，`index.html` 为入口文件

你也可以安装 `Ionic` 之后，运行 `ionic serve` 进行开发或预览   
它会监控文件的修改并即时编译，它还会启动一个 Web Service，默认通过 `htpp://localhost:8100` 即可打开 Web App


### 构建 Hybrid App

```
$ cd HeyCommunity/frontend
$ npm install
$ npm install -g ionic cordova                  ## 安装 ionic 和 cordova
$ ionic state reset                             ## 重置 platforms 和 plugins
$ ionic build android                           ## 构建 Android App (需要做好 Android SDK 等配置)
$ ionic build ios                               ## 构建 iOS App (只工作在 OS X 平台)
$ open platforms/ios/HeyCommunity.xcodeproj     ## Optional, use xcode to open HeyCommunity.xcodeproj, the simulator runs, real machine test, upload to the AppStore
```

成功 `build android` 之后，构建的 apk 文件会在 `HeyCommunity/frontend/platforms/android/build/outputs` 目录中   
成功 `build ios` 之后，会生成一个 xcodeproj 存放在 `HeyCommunity/frontend/platforms/ios` 目录中，使用 Xcode 打开目录中的 `HeyCommunity.xcodeproj`，即可通过 Xcode 去做真机调试或者上传到 AppStore

在 `HeyCommunity/frontend/config.xml` 文件中定义了 App Name / Bundle ID / Version 和一些插件的参数，你可以为你自己的 App 做相关修改




## 运行

我们目前主要支持 Apache 服务器（能够运行在 Nginx 中，但目前暂不提供支持）。现在我们通过 Apache 把后端和 Web App 运行起来用于生产

### 开启 Apache Rewrite Module & 设置 AllowOverride All
1. 打开 `httpd.conf` 把 `LoadModule rewrite_module path/mod_rewrite.so` 前面的 `#` 注释删除   
2. 定义一个虚拟主机，`DocumentRoot` 设为 `HeyCommunity` 目录；并设置 HeyCommunity 目录 AllowOverride All。参考下面的配置代码
3. 重启 Apache

```
<VirtualHost *:80>
    ServerAdmin hi@hey-community.com
    DocumentRoot "/var/www/HeyCommunity"
    ServerAlias demo.hey-community.local

    ErrorLog "/var/log/apache2/all-error_log"
    CustomLog "/var/log/apache2/all-access_log" common

    <Directory "/var/www/HeyCommunity">
        Options Indexes FollowSymLinks Multiviews
        MultiviewsMatch Any

        AllowOverride All
    </Directory>
</VirtualHost>
```

现在我们已经通过 Apache 把后端 API 和 Web App 部署好了，那么在浏览器中打开 `htpp://demo.hey-community.local` 即可体验和预览 HeyCommunity 在浏览器中的运行   
IOS 和 Android 设备上的运行暂不做介绍，如有需要请查看 Ionic 官方文档
