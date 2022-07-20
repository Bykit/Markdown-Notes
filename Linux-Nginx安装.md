### 1、解决依赖

> 预先安装额外的依赖

```sh
yum install gcc-c++
yum install -y pcre pcre-devel
yum install -y zlib zlib-devel
yum install -y openssl openssl-devel

# 安装 nginx 需要先将官网下载的源码进行编译，编译依赖 gcc 环境

# Nginx的Rewrite模块和HTTP核心模块会使用到PCRE正则表达式语法
# 这里需要安装两个安装包pcre和pcre-devel，第一个安装包提供编译版本的库，而第二个提供开发阶段的头文件和编译项目的源代码

# zlib库提供了开发人员的压缩算法，在Nginx的各种模块中需要使用gzip压缩

#  nginx不仅支持 http协议，还支持 https（即在 ssl 协议上传输 http）, 如果使用了 https，需要安装 OpenSSL 库
```

### 2、解压编译安装

```sh
# 安装包解压，安装包去官网下载
mkdir /usr/local/nginx
cd /usr/local/nginx
tar zxvf /root/nginx-1.17.10.tar.gz   

# 编译安装NGINX
# 安装完成后，Nginx的可执⾏⽂件位置位于/usr/local/nginx/sbin/nginx
cd nginx-1.17.10
./configure
make
make install
```

### 3、配置环境变量

```sh
vi /etc/profile

#添加以下内容
export NIGINX_HOME=/usr/local/nginx
export PATH=$PATH:$NIGINX_HOME/sbin

source /etc/profile
```

### 4、nginx管理命令

```sh
nginx
nginx -s stop           #停⽌Nginx
nginx -s reload         #修改了配置⽂件后重新加载Nginx
nginx -c /usr/local/nginx/conf/nginx.conf     #这个不理解
ps -ef | grep nginx      #查看nginx进程
```

### 5、nginx.conf配置文件

> 文件路径：/usr/local/nginx/conf/nginx.conf

```sh

#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

	
    server {
    
    	#监听的端口
        listen       80;
        server_name  localhost;

        #charset koi8-r;
				
	#正常日志的输出目录
        #access_log  logs/host.access.log  main;

        location / {
            #访问根路径映射的文件夹
            root   html;
            
            #访问的url不添加文件时加载的首页
            index  index.html index.htm;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }


    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}

}

```



 





