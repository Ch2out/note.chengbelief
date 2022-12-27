# nginx教程

## 启动、重启、关闭指令

### 启动指令

> cd nginx文件的位置（服务器中则就是nginx在服务器中的位置）
> .\nginx -c .\conf\nginx.conf

### 重启指令

> cd nginx文件的位置（服务器中则就是nginx在服务器中的位置）
> .\nginx -s reload 

### 关闭指令

> cd nginx文件的位置（服务器中则就是nginx在服务器中的位置）
> .\nginx -s stop -c .\conf\nginx.conf

### 服务器配置指令

``` xml
 sudo nginx -t -c 服务器中nginx里面的server.nginx.conf
 sudo nginx -c 服务器中nginx里面的server.nginx.conf
 sudo nginx -s stop -c 服务器中nginx里面的server.nginx.conf
# 重新加载nginx配置文件然后重启nginx服务
 sudo nginx -s reload 
```

## 配置解析

### server配置

基础server配置

``` xml
个人电脑：
此server当我访问127.0.0.1:80端口时对应server_name 和 listen 
当请求路径为/时 访问本电脑上的location/下的root的路径地址。
当访问路径为index 则时文件夹下的index.html文件。
 server {
   listen   80;
   server_name  127.0.0.1;
   charset utf-8;
   location / {
     root  F:\Users\chengbelief\Desktop\vue\vue-dome\dist;
     index  index.html ;
     }
 }
```

#### 怎么实现两个访问不同的域名，却同时占用80端口，访问的页面也不同。（实现服务器或个人电脑nginx同时配置多个域名）

个人电脑：只需要在C:\Windows\System32\drivers\etc\hosts 文件同时配置两个同域名的值。因为他两个域名的指向任然相同但指向的文件位置不同。所以可以实现。
<hr>
服务器:服务器通过在阿里云中（购买域名时）对不同的域名通过配置DNS解析时指向同一台服务器。在我们到服务器中的nginx配置中进行配置即可。

#### 服务器如何配置从http请求转成https请求

``` yaml
   #chengbelief.top配置 
  server {
    listen 80;
    server_name chengbelief.top;
    server_name www.chengbelief.top;
    charset utf-8;
    #作用当访问请求为http请求时，自动将他变成https请求 
    rewrite ^(.*)$ https://${server_name}$1 permanent;
  }
  #chengbelief.top的ssl配置 
  server {
    #端口号为443号 
    listen 443 ssl;
    server_name chengbelief.top;
    #ssl证书位置（根据证书在服务器中的位置来选择）
    ssl_certificate chengbelief.top.pem;
    #ssl证书key （根据证书在服务器中的位置来选择）
    ssl_certificate_key chengbelief.top.key;
    ssl_session_timeout 5m;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;
    charset utf-8;
    # 静态主网站
    location / {
      # 服务器文件夹真实夹
      root /home/chenghan/git/chengbelief.top.server/chengbelief.top;
      index index.html;
      expires -1;
    }
  }
```

#### 服务器二级子域名

为什么二级子域名是在前面te.域名完成
> 因为chengbelief.top/ 后面的是真实的路径

```yaml
   # te.chengblief.top配置
  server {
    # 访问端口号
    listen 80;
    # 需要访问的名城
    server_name te.chengbelief.top;
    charset utf-8;
    location / {
      # 服务器中真实的地址
      root /home/chenghan/git/chengbelief.top.server/te.chengbelief.top;
      index index.html;
      expires -1;
    }
  }
```

#### 使用反代处理跨域问题（没有写完）

可以在放置war的服务器上面配置nginx

``` yaml
server {
        listen       80;
        server_name  huhuiyu.shop;
        charset utf-8;
        location /api/ {
            proxy_pass https://huhuiyu.top/teach_project_service/;
        }
        # 通过虚拟目录发布vue项目
        location /vue/ {
            alias   D:\\nginx-2001\\vue02\\dist\\;
            index  index.html;
            # 必须指定到虚拟目录中的index.html
            try_files $uri $uri/ /vue/index.html;
        }
    }
```

#### 虚拟目录（没有写完）