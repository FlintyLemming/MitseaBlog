+++
author = "FlintyLemming"
title = "使用 proxy_pass 反代"
slug = "34a1ad27d1c7465c84df4daa12802301"
date = "2020-03-07"
description = ""
categories = ["Linux", "Network"]
tags = ["Linux", "Nginx"]
image = "https://img.mitsea.com/blog/posts/2020/03/%E4%BD%BF%E7%94%A8%20proxy_pass%20%E5%8F%8D%E4%BB%A3/title.avif"
+++

好比我利用家庭公网搭建了一个小网站，设置了解析和端口，现在的地址是 [site.name.com:8086](http://site.name.com:8086)，毕竟家宽肯定 443 和 80 是不能用的。那我现在想通过一个公网机器，并且这个机器是开放 443 端口的，来反代家宽上的网站，以达到去掉端口后缀的效果，就要用到 proxy_pass 这个功能。目标就是通过 [web.name.com](http://web.name.com) 就能直接访问到 site.name.com:8086 上的内容。

机器环境是 Ubuntu 18.04.1 LTS

1. 首先更新软件包列表

    ```bash
    sudo apt-get update
    ```

2. 安装 Nginx

    ```bash
    sudo apt-get install nginx
    ```

3. 添加 vhost，虽然就一个站点，但我还是习惯添加 vhost 单独给写出来

    创建站点文件夹

    ```bash
    sudo mkdir -p /var/www/<任意名称>
    ```

    下文中我使用 /var/www/web.name.com 这个路径为例

4. 使用 vim 编辑器编辑 /etc/nginx/nginx.conf

    ```bash
    sudo vim /etc/nginx/nginx.conf
    ```

    按下 i 键进行编辑

    在 http{} 里添加一行（这个一般都有）

    ```bash
    include /etc/nginx/conf.d/*.conf;
    ```

    按下 Esc，并输入 :wq 回车保存并退出

5. 在上述文件夹中添加配置文件，直接 vim 就可以新建一个文件并开始编辑

    ```bash
    sudo vim /etc/nginx/conf.d/web.name.com.conf
    ```

    配置文件如下

    ```bash
    server
    {
        listen 443 ssl http2;
        server_name web.name.com;
        ssl on;
        ssl_certificate   /etc/nginx/ssl/web.name.com.crt;
        ssl_certificate_key  /etc/nginx/ssl/web.name.com.key;
        ssl_session_timeout 5m;
        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_prefer_server_ciphers on;
        access_log off;
        location / {
            proxy_pass https://site.name.com:8086/;
      }
    }
    ```

    你需要把你的证书命名为 web.name.com 然后放到 /etc/nginx/ssl 目录下

6. 重启 nginx 服务

    ```bash
    nginx -s reload
    ```

7. 然后就能通过访问 [https://web.name.com](https://web.name.com) 访问到 [https://site.name.com:](https://site.name.com:9096)8086 的内容了