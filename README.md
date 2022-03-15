# sub-web-modify
基于项目[CareyWang/sub-web](https://github.com/CareyWang/sub-web)和[youshandefeiyang/sub-web-modify](https://github.com/youshandefeiyang/sub-web-modify)，在原有的跟随主题切换明/暗主题的基础上，删除广告弹窗，删除高级功能切换按钮，直接显示全部选项。删除youtube视频、TG频道等跳转，页面更加简介美观。删除上传自定义配置功能，内带大量分组足够满足使用。修改了网页图标为一直可爱的猫咪，更符合`Clash`主题。本项目主要是个人使用为主，不保证更新，本人纯小白，对`css`，`html`，`js`以及`vue.js`框架完全不了解，只为了满足有强迫症的你我能使用一个简洁的转换前端。

教程部分参考：[clash订阅转换搭建 - liuliのsite (back2me.cn)](https://www.back2me.cn/skills/clash.html)和[Sub-Web搭建教程！](https://www.v2rayssr.com/sub-web.html)

## 效果预览：
### 食用方法【以Linux(ubuntu 20.04 LTS)为例】：
1.安装 [node](https://blog.csdn.net/achabuhecha/article/details/111400068) 和 [yarn](https://classic.yarnpkg.com/en/docs/install#debian-stable) 和 [git](https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-%E5%AE%89%E8%A3%85-Git)

```shell
sudo apt update -y
sudo apt install -y curl wget sudo nodejs git
sudo apt install npm
sudo npm install -g cnpm --registry=https://registry.npm.taobao.org
sudo cnpm install -g yarn
sudo apt install git
## 验证是否安装成功
sudo node -v
sudo yarn --version
sudo git --version
```

2.拉取并进入目录

```shell
sudo git clone git://github.com/fuxenn/sub-web-clean.git
cd sub-web-clean
```

3. 在项目目录中安装构建依赖项，构建的过程稍微有点长

```shell
sudo yarn install
```

4. 调试和本地运行

```shell
sudo yarn serve
```

出现下图则表示前端调试模式启动成功

![预览](https://cdn.back2me.cn/2020/11/02/70de43e5ca4da.png)

这时，我们浏览器访问 `http://服务器ip:8080/` 应该可以进行前端 sub-web-clean 的预览了。

（记住8080端口的防火墙和安全组要开放）

5. 如果需要添加自己的后端服务，可以找到目录下的`/sub-web/src/views/Subconverter.vue`文件，找到`customBackend: {}`数组，在其中加入自己的后端地址。调试完成后就可以构建项目了。

```shell
sudo yarn build
```

6. build成功后，需要安装nginx并正确配置，以下为nginx server部分配置，可以参考一下【这块建议新手使用宝塔面板等自动化运维工具，直接将网站目录更改为/sub-web-clean/dist即可】！

```shell
server {
    listen 80;
    server_name 你的域名或IP;
    root /home/sub-web-modify/dist;
    index index.html index.htm;
    error_page 404 /index.html;
    gzip on; #开启gzip压缩
    gzip_min_length 1k; #设置对数据启用压缩的最少字节数
    gzip_buffers 4 16k;
    gzip_http_version 1.0;
    gzip_comp_level 6; #设置数据的压缩等级,等级为1-9，压缩比从小到大
    gzip_types text/plain text/css text/javascript application/json application/javascript application/x-javascript application/xml; #设置需要压缩的数据格式
    gzip_vary on;
    location ~* \.(css|js|png|jpg|jpeg|gif|gz|svg|mp4|ogg|ogv|webm|htc|xml|woff)$ {
        access_log off;
        add_header Cache-Control "public,max-age=30*24*3600";
    }
}
```
