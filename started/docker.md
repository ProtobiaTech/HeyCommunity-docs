# Docker 部署

Docker Image 托管在[这里](https://hub.daocloud.io/repos/e9aa4c04-33ac-4bc4-99fa-fb727c7acc11)，你可以参考以下 compose 规则进行自定义部署 HeyCommunity

```
app:
  image: daocloud.io/rodv2/hey-community:latest
  environment:
    - LOCALE=zh-CN
    - HC_ENV=product
    - COMMUNITY_NAME=New Community
    - ADMIN_NICKNAME=Admin
    - ADMIN_EMAIL=admin@hey-community.com
    - ADMIN_PASSWORD=hey community
    - WECHAT_APPID=your_wechatpa_app_id
    - WECHAT_SECRET=your_wechatpa_app_secret
    - WECHAT_TEMP_NOTICE_ID=your_wechatpa_temp_id
  links:
    - db:MYSQL
  volumes:
    - /data/HeyCommunityCloudStorage:/app/backend/public/uploads
  ports:
    - "80"
  restart: always
db:
  image: mysql:5.6
  environment:
    - MYSQL_DATABASE=prod_heycommunity
    - MYSQL_ROOT_PASSWORD=Root
  volumes:
    - /data/HeyCommunityCloudDBStorage:/var/lib/mysql
  ports:
    - "3306"
  restart: always
```
