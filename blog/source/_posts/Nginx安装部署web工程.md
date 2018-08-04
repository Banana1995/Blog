---
title: Nginx安装部署web工程

categories: "Linux环境"

tags: "Nginx"
---



## Nginx安装

### 1. 下载依赖包

- 如果你的服务器可以连接网络的话可以直接通过命令的方式下载tar包

  1. 下载PCRE库

     `wget ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.38.tar.gz  `

  2. 下载zlib库

     `wget http://zlib.net/zlib-1.2.10.tar.gz `

  3. 下载OpenSSL库

     `wget https://www.openssl.org/source/openssl-1.0.2o.tar.gz `

  <!--more-->

- 如果你的服务器无法联网，可以自行去上面的`wget`命令后面的地址下载对应的tar包文件，ftp路径也可以直接通过浏览器打开，再将他们传到服务器中。

### 2. 安装依赖包

- 安装PCRE库

  - `tar -zxvf pcre-8.37.tar.gz ` 先使用命令解压tar包

  - `cd pcre-8.38` 进去解压后的文件目录

  - `./configure` 运行初始化脚本

    **如果此时你不是用root用户安装，或者希望指定安装根路径，可以使用`--prefix=`参数来指定安装路径** 

    如：使用`./configure --prefix=/home/test/pcre`命令来指定将pcre安装到`/home/tese/pcre`目录下

  - `make` 使用make命令尝试编译

  - `make install` 编译安装

- 安装zlib库

  - `tar -zxvf zlib-1.2.10.tar.gz ` 先使用命令解压tar包

  - `cd zlib-1.2.10` 进去解压后的文件目录

  - `./configure` 运行初始化脚本

    **如果此时你不是用root用户安装，或者希望指定安装根路径，可以使用`--prefix=`参数来指定安装路径** 

    如：使用`./configure --prefix=/home/test/zlib`命令来指定将zlib安装到`/home/test/zlib`目录下

  - `make` 使用make命令尝试编译

  - `make install` 编译安装

- 安装OpenSSL库

  - `tar -zxvf openssl-1.0.2o.tar.gz ` 使用命令解压tar包即可

### 3. 安装Nginx

#### 3.1 安装步骤

- 先下载Nginx的tar包，我这里选择的是最新的稳定版`nginx-1.14.0.tar.gz`

  跟上面安装依赖包一样，如果你的服务器可以联网的话建议使用`wget http://nginx.org/download/nginx-1.14.0.tar.gz `命令来下载tar包；如果不能联网的话建议自己通过浏览器访问http地址下载

- `tar -zxvf nginx-1.14.0.tar.gz ` 解压Nginx的tar包

- `cd nginx-1.14.0` 进入解压后的目录

- `./configure`运行初始化脚本

  **注意：Nginx默认的安装路径是`/usr/local/nginx`,如果你不是使用root用户的话，就不能使用该路径。这里同样可以使用`--prefix=`参数来指定安装路径** 

  同时，你可以使用`--with-pcre=/home/test/pcre-8.38`来指定pcre安装路径

  使用`--with-zlib=/home/test/zlib`来指定zlib的安装路径

- `make` 使用make命令尝试编译

- `make install` 编译安装

- `cd sbin` 进入sbin目录下

- `./nginx` 启动Nginx  同时可以使用 `./nginx -s reload` 重启Nginx 使用`./nginx -s stop` 关停Nginx

#### 3.2 Nginx常用编译选项

> make是用来编译的，它从Makefile中读取指令，然后编译。
>
> make install是用来安装的，它也从Makefile中读取指令，安装到指定的位置。
>
> configure命令是用来检测你的安装平台的目标特征的。它定义了系统的各个方面，包括nginx的被允许使用的连接处理的方法，比如它会检测你是不是有CC或GCC，并不是需要CC或GCC，它是个shell脚本，执行结束时，它会创建一个Makefile文件。nginx的configure命令支持以下参数：
>
> - `--prefix=*path*`    定义一个目录，存放服务器上的文件 ，也就是nginx的安装目录。默认使用 `/usr/local/nginx。`
>
> - `--sbin-path=*path*` 设置nginx的可执行文件的路径，默认为  `*prefix*/sbin/nginx`.
>
> - `--conf-path=*path*`  设置在nginx.conf配置文件的路径。nginx允许使用不同的配置文件启动，通过命令行中的-c选项。默认为`*prefix*/conf/nginx.conf`.
>
> - `--pid-path=*path*  设置nginx.pid文件，将存储的主进程的进程号。安装完成后，可以随时改变的文件名 ， 在nginx.conf配置文件中使用 PID指令。默认情况下，文件名 为``*prefix*/logs/nginx.pid`.
>
> - `--error-log-path=*path*` 设置主错误，警告，和诊断文件的名称。安装完成后，可以随时改变的文件名 ，在nginx.conf配置文件中 使用 的error_log指令。默认情况下，文件名 为`*prefix*/logs/error.log`.
>
> - `--http-log-path=*path*`  设置主请求的HTTP服务器的日志文件的名称。安装完成后，可以随时改变的文件名 ，在nginx.conf配置文件中 使用 的access_log指令。默认情况下，文件名 为`*prefix*/logs/access.log`.
>
> - `--user=*name*`  设置nginx工作进程的用户。安装完成后，可以随时更改的名称在nginx.conf配置文件中 使用的 user指令。默认的用户名是nobody。
>
> - `--group=*name*`  设置nginx工作进程的用户组。安装完成后，可以随时更改的名称在nginx.conf配置文件中 使用的 user指令。默认的为非特权用户。
>
> - `--with-select_module` `--without-select_module 启用或禁用构建一个模块来允许服务器使用select()方法。该模块将自动建立，如果平台不支持的kqueue，epoll，rtsig或/dev/poll。`
>
> - `--with-poll_module` `--without-poll_module` 启用或禁用构建一个模块来允许服务器使用poll()方法。该模块将自动建立，如果平台不支持的kqueue，epoll，rtsig或/dev/poll。
>
> - `--without-http_gzip_module` — 不编译压缩的HTTP服务器的响应模块。编译并运行此模块需要zlib库。
>
> - `--without-http_rewrite_module`  不编译重写模块。编译并运行此模块需要PCRE库支持。
>
> - `--without-http_proxy_module` — 不编译http_proxy模块。
>
> - `--with-http_ssl_module` — 使用https协议模块。默认情况下，该模块没有被构建。建立并运行此模块的OpenSSL库是必需的。
>
> - `--with-pcre=*path*` — 设置PCRE库的源码路径。PCRE库的源码（版本4.4 - 8.30）需要从PCRE网站下载并解压。其余的工作是Nginx的./ configure和make来完成。正则表达式使用在location指令和 ngx_http_rewrite_module 模块中。
>
> - `--with-pcre-jit` —编译PCRE包含“just-in-time compilation”（1.1.12中， pcre_jit指令）。
>
> - `--with-zlib=*path*` —设置的zlib库的源码路径。要下载从 zlib（版本1.1.3 - 1.2.5）的并解压。其余的工作是Nginx的./ configure和make完成。ngx_http_gzip_module模块需要使用zlib 。
>
> - `--with-cc-opt=*parameters*` — 设置额外的参数将被添加到CFLAGS变量。例如,当你在FreeBSD上使用PCRE库时需要使用:`--with-cc-opt="-I /usr/local/include。`.如需要需要增加 `select()支持的文件数量`:`--with-cc-opt="-D FD_SETSIZE=2048".`
>
> - `--with-ld-opt=*parameters*` —设置附加的参数，将用于在链接期间。例如，当在FreeBSD下使用该系统的PCRE库,应指定:`--with-ld-opt="-L /usr/local/lib".`
>
> - 典型实例(下面为了展示需要写在多行，执行时内容需要在同一行)
>
>   ```
>   ./configure
>       --sbin-path=/usr/local/nginx/nginx
>       --conf-path=/usr/local/nginx/nginx.conf
>       --pid-path=/usr/local/nginx/nginx.pid
>       --with-http_ssl_module
>       --with-pcre=../pcre-4.4
>       --with-zlib=../zlib-1.1.3
>   ```

## 部署web工程

### 1. 关于Nginx配置文件

在部署web工程前，我们需要了解Nginx的配置文件。Nginx的配置文件存放在`nginx/conf/nginx.conf`。我们需要打开这个文件根据自己的web工程需求配置这个文件。

> 在nginx配置文件中主要分为四部分：`main` 全局设置，`server`主机设置，`upstream`（上游服务器设置，主要为反向代理、负载均衡相关配置）和 `location`（URL匹配特定位置后的设置） main部分设置的指令将影响其它所有部分的设置；server部分的指令主要用于指定虚拟主机域名、IP和端口；upstream的指令用于设置一系列的后端服务器，设置反向代理及后端服务器的负载均衡；location部分用于匹配网页位置（比如，根目录“/”,“/images”,等等）。他们之间的关系式：server继承main，location继承server；upstream既不会继承指令也不会被继承。它有自己的特殊指令，不需要在其他地方的应用。 

先贴一个配置文件，再来按照这个文件进行说明

```
#user  nobody;

#在配置文件的顶级main部分，worker角色的工作进程的个数，master进程是接收并分配请求给worker处理。这个数值简单一点可以设置为cpu的核数grep ^processor /proc/cpuinfo | wc -l，也是 auto 值，如果开启了ssl和gzip更应该设置成与逻辑CPU数量一样甚至为2倍，可以减少I/O操作。如果nginx服务器还有其它服务，可以考虑适当减少。

worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
#写在events部分。每一个worker进程能并发处理（发起）的最大连接数（包含与客户端或后端被代理服务器间等所有连接数）。nginx作为反向代理服务器，计算公式 最大连接数 = worker_processes * worker_connections/4，所以这里客户端最大连接数是1024，这个可以增到到8192都没关系，看情况而定，但不能超过后面的worker_rlimit_nofile。当nginx作为http服务器时，计算公式里面是除以2。
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';
    #access_log  logs/access.log  main;
	#开启高效文件传输模式，sendfile指令指定nginx是否调用sendfile函数来输出文件，	减少用户空间到内核空间的上下文切换。对于普通应用设为 on，如果用来进行下载等应用	磁盘IO重负载应用，可设置为off，以平衡磁盘与网络I/O处理速度，降低系统的负载。
    sendfile        on;
    # tcp_nopush     on;
	#长连接超时时间，单位是秒。长连接请求大量小文件的时候，可以减少重建连接的开销，	但假如有大文件上传，65s内没上传完成会导致失败。如果设置时间过长，用户又多，长时	 间保持连接会占用大量资源。
    keepalive_timeout  65;

  # gzip压缩功能设置
	# 开启gzip压缩输出，减少网络传输
    gzip on;
	#设置允许压缩的页面最小字节数，页面字节数从header头得content-length中进行获取。默认值是20。建议设置成大于1k的字节数，小于1k可能会越压越大
    gzip_min_length 1k;
	#设置系统获取几个单位的缓存用于存储gzip的压缩结果数据流。4 16k代表以16k为单位，安装原始数据大小以16k为单位的4倍申请内存。
    gzip_buffers    4 16k;
	#gzip压缩比，1压缩比最小处理速度最快，9压缩比最大但处理速度最慢(传输快但比较消耗cpu)
    gzip_comp_level 6;
	#匹配mime类型进行压缩，无论是否指定,”text/html”类型总是会被压缩的。
    gzip_types text/html text/plain text/css text/javascript application/json application/javascript application/x-javascript application/xml;
    #和http头有关系，会在响应头加个 Vary: Accept-Encoding ，可以让前端的缓存服务器缓存经过gzip压缩的页面，例如，用Squid缓存经过Nginx压缩的数据。。
	gzip_vary on;
  
  # http_proxy 设置
	#允许客户端请求的最大单文件字节数。如果有上传较大文件，请设置它的限制值
    client_max_body_size   10m;
	#缓冲区代理缓冲用户端请求的最大字节数
    client_body_buffer_size   128k;
	#nginx跟后端服务器连接超时时间(代理连接超时)
    proxy_connect_timeout   75;
	#连接成功后，与后端服务器两个成功的响应操作之间超时时间(代理接收超时)
    proxy_read_timeout   75;
	#设置代理服务器（nginx）从后端realserver读取并保存用户头信息的缓冲区大小，默认		与proxy_buffers大小相同，其实可以将这个指令值设的小一点
    proxy_buffer_size   4k;
	#proxy_buffers缓冲区，nginx针对单个连接缓存来自后端realserver的响应，网页平均	  在32k以下的话，这样设置
    proxy_buffers   4 32k;
	#高负荷下缓冲大小（proxy_buffers*2）
    proxy_busy_buffers_size   64k;
	#当缓存被代理的服务器响应到临时文件时，这个选项限制每次写临时文件的大小。		proxy_temp_path（可以在编译的时候）指定写到哪那个目录。。
    proxy_temp_file_write_size  64k;
	#指定将上面的临时文件写到哪那个目录。
    proxy_temp_path   /usr/local/nginx/proxy_temp 1 2;

  # 设定负载均衡后台服务器列表 
    upstream  arc  { 
              #ip_hash; 
              server   192.168.10.100:8080 max_fails=2 fail_timeout=30s ;  
              server   192.168.10.101:8080 max_fails=2 fail_timeout=30s ;  
    }

  # 很重要的虚拟主机配置
    server {
		#虚拟主机监听的端口
        listen       8001;
		#服务器名
        server_name  localhost;

        #charset utf-8;
        #access_log  logs/host.access.log  main;

        #对 / 所有做负载均衡+反向代理
        location / {
		   #定义服务器的默认网站根目录位置。
            root   html;
		   #定义路径下默认访问的文件名
            index  index.jsp index.html index.htm;
		   #请求转向arc定义的服务器列表，即反向代理，对应upstream负载均衡器。
            proxy_pass        http://arc;
		   #下面这几个就这么设置吧  具体的我也不清楚
            proxy_redirect off;
            # 后端的Web服务器可以通过X-Forwarded-For获取用户真实IP
            proxy_set_header  Host  $host;
            proxy_set_header  X-Real-IP  $remote_addr;  
            proxy_set_header  X-Forwarded-For  $proxy_add_x_forwarded_for;
            proxy_next_upstream error timeout invalid_header http_500 http_502 http_503 http_504;
            
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }

  ## 其它虚拟主机，server 指令开始
}
```

### 2. 部署前端工程

将web项目上传到Nginx的安装目录中的`html`文件夹中。修改`nginx.conf`配置文件。

## 遇到的问题

1. web端不能访问

   检查防火墙是否关闭！关闭防火墙：`service iptables stop`

2. 非root用户报出`bind() to 0.0.0.0:80 failed (13:Permission denied)`错误

   这是由于非root用户启动时，`nginx.conf`文件中配置的端口为`80`，而在Linux中只有root用户才能使用1024以下的端口。所以只要讲配置文件中的端口修改为1024以上即可。

## 参考链接

[Nginx安装](http://www.nginx.cn/install)

[Nging下部署项目，配置文件修改](https://blog.csdn.net/tototuzuoquan/article/details/47381907)

[nginx服务器安装及配置文件详解](http://seanlook.com/2015/05/17/nginx-install-and-config/)