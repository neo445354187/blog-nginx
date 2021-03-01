# nginx

## 参考资料
<http://bbs.linuxtone.org/thread-25588-1-1.html>
<http://shouce.jb51.net/nginx/left.html>
<http://tool.oschina.net/apidocs/apidoc?api=nginx-zh>
<http://www.nginx.cn/doc/>

## 网上最全面nginx教程（近100篇文章整理）

### nginx安装

1、必要软件准备

安装pcre
为了支持rewrite功能，我们需要安装pcre
```
# yum install pcre* //如过你已经装了，请跳过这一步
```

安装openssl
需要ssl的支持，如果不需要ssl支持，请跳过这一步

```
# yum install openssl*
```

2、安装nginx

执行如下命令：
```
# ./configure --prefix=/usr/local/nginx-1.5.1 \
--with-http_ssl_module --with-http_spdy_module \
--with-http_stub_status_module --with-pcre
```

--with-http_stub_status_module：支持nginx状态查询
--with-http_ssl_module：支持https
--with-http_spdy_module：支持google的spdy,想了解请百度spdy,这个必须有ssl的支持
--with-pcre：为了支持rewrite重写功能，必须制定pcre
最后输出如下内容，表示configure OK了。

```
checking for zlib library ... found
 creating objs/Makefile
Configuration summary
 + using system PCRE library
 + using system OpenSSL library
 + md5: using OpenSSL library
 + sha1: using OpenSSL library
 + using system zlib library
nginx path prefix: "/usr/local/nginx-1.5.1"
 nginx binary file: "/usr/local/nginx-1.5.1/sbin/nginx"
 nginx configuration prefix: "/usr/local/nginx-1.5.1/conf"
 nginx configuration file: "/usr/local/nginx-1.5.1/conf/nginx.conf"
 nginx pid file: "/usr/local/nginx-1.5.1/logs/nginx.pid"
 nginx error log file: "/usr/local/nginx-1.5.1/logs/error.log"
 nginx http access log file: "/usr/local/nginx-1.5.1/logs/access.log"
 nginx http client request body temporary files: "client_body_temp"
 nginx http proxy temporary files: "proxy_temp"
 nginx http fastcgi temporary files: "fastcgi_temp"
 nginx http uwsgi temporary files: "uwsgi_temp"
 nginx http scgi temporary files: "scgi_temp"
# make //确定你的服务器有安装make，如果没有安装请执行yum install make
# make install
```

3、启动、关闭、重置nginx

启动：直接执行以下命令,nginx就启动了,不需要改任何配置文件,nginx配置多域名虚拟主机请参考后续文章.
```
/usr/local/nginx-1.5.1/sbin/nginx
```
试试访问：我这边不贴图，直接使用curl命令来读取web信息
```
[root@ns conf]# curl -s http://localhost | grep nginx.com
nginx.com.
```

关闭：
```
/usr/local/nginx-1.5.1/sbin/nginx -s stop
```

重置：当你有修改配置文件的时候，只需要reload以下即可
```
/usr/local/nginx-1.5.1/sbin/nginx -s reload
```
整个nginx的安装就到这里结束了。
转载请注明出处：<http://www.ttlsa.com/html/1548.html>

### nginx 编译参数详解

标题是不是很欠揍,个人认为确实值得一看,如果你不了解nginx，或者你刚学nginx，或者已经使用nginx一段时间了。但是nginx很多参数你还没去了解，nginx有很多你不知道的用处。不废话，上内容。内容是从网络上摘抄的。
内容有些多，一眼看来难免头昏脑胀，但坚持看完，相信你一定会有所收获。
nginx参数：

--prefix= 指向安装目录

--sbin-path 指向（执行）程序文件（nginx）

--conf-path= 指向配置文件（nginx.conf）

--error-log-path= 指向错误日志目录

--pid-path= 指向pid文件（nginx.pid）

--lock-path= 指向lock文件（nginx.lock）（安装文件锁定，防止安装文件被别人利用，或自己误操作。）

--user= 指定程序运行时的非特权用户

--group= 指定程序运行时的非特权用户组

--builddir= 指向编译目录

--with-rtsig_module 启用rtsig模块支持（实时信号）

--with-select_module 启用select模块支持（一种轮询模式,不推荐在高载环境下使用）禁用：--without-select_module

--with-poll_module 启用poll模块支持（功能与select相同，与select特性相同，为一种轮询模式,不推荐在高载环境下使用）

--with-file-aio 启用file aio支持（一种APL文件传输格式）

--with-ipv6 启用ipv6支持

--with-http_ssl_module 启用ngx_http_ssl_module支持（使支持https请求，需已安装openssl）

--with-http_realip_module 启用ngx_http_realip_module支持（这个模块允许从请求标头更改客户端的IP地址值，默认为关）

--with-http_addition_module 启用ngx_http_addition_module支持（作为一个输出过滤器，支持不完全缓冲，分部分响应请求）

--with-http_xslt_module 启用ngx_http_xslt_module支持（过滤转换XML请求）

--with-http_image_filter_module 启用ngx_http_image_filter_module支持（传输JPEG/GIF/PNG 图片的一个过滤器）（默认为不启用。gd库要用到）

--with-http_geoip_module 启用ngx_http_geoip_module支持（该模块创建基于与MaxMind GeoIP二进制文件相配的客户端IP地址的ngx_http_geoip_module变量）

--with-http_sub_module 启用ngx_http_sub_module支持（允许用一些其他文本替换nginx响应中的一些文本）

--with-http_dav_module 启用ngx_http_dav_module支持（增加PUT,DELETE,MKCOL：创建集合,COPY和MOVE方法）默认情况下为关闭，需编译开启

--with-http_flv_module 启用ngx_http_flv_module支持（提供寻求内存使用基于时间的偏移量文件）

--with-http_gzip_static_module 启用ngx_http_gzip_static_module支持（在线实时压缩输出数据流）

--with-http_random_index_module 启用ngx_http_random_index_module支持（从目录中随机挑选一个目录索引）

--with-http_secure_link_module 启用ngx_http_secure_link_module支持（计算和检查要求所需的安全链接网址）

--with-http_degradation_module  启用ngx_http_degradation_module支持（允许在内存不足的情况下返回204或444码）

--with-http_stub_status_module 启用ngx_http_stub_status_module支持（获取nginx自上次启动以来的工作状态）

--without-http_charset_module 禁用ngx_http_charset_module支持（重新编码web页面，但只能是一个方向--服务器端到客户端，并且只有一个字节的编码可以被重新编码）

--without-http_gzip_module 禁用ngx_http_gzip_module支持（该模块同-with-http_gzip_static_module功能一样）

--without-http_ssi_module 禁用ngx_http_ssi_module支持（该模块提供了一个在输入端处理处理服务器包含文件（SSI）的过滤器，目前支持SSI命令的列表是不完整的）

--without-http_userid_module 禁用ngx_http_userid_module支持（该模块用来处理用来确定客户端后续请求的cookies）

--without-http_access_module 禁用ngx_http_access_module支持（该模块提供了一个简单的基于主机的访问控制。允许/拒绝基于ip地址）

--without-http_auth_basic_module禁用ngx_http_auth_basic_module（该模块是可以使用用户名和密码基于http基本认证方法来保护你的站点或其部分内容）

--without-http_autoindex_module 禁用disable ngx_http_autoindex_module支持（该模块用于自动生成目录列表，只在ngx_http_index_module模块未找到索引文件时发出请求。）

--without-http_geo_module 禁用ngx_http_geo_module支持（创建一些变量，其值依赖于客户端的IP地址）

--without-http_map_module 禁用ngx_http_map_module支持（使用任意的键/值对设置配置变量）

--without-http_split_clients_module 禁用ngx_http_split_clients_module支持（该模块用来基于某些条件划分用户。条件如：ip地址、报头、cookies等等）

--without-http_referer_module 禁用disable ngx_http_referer_module支持（该模块用来过滤请求，拒绝报头中Referer值不正确的请求）

--without-http_rewrite_module 禁用ngx_http_rewrite_module支持（该模块允许使用正则表达式改变URI，并且根据变量来转向以及选择配置。如果在server级别设置该选项，那么他们将在 location之前生效。如果在location还有更进一步的重写规则，location部分的规则依然会被执行。如果这个URI重写是因为location部分的规则造成的，那么 location部分会再次被执行作为新的URI。 这个循环会执行10次，然后Nginx会返回一个500错误。）

--without-http_proxy_module 禁用ngx_http_proxy_module支持（有关代理服务器）

--without-http_fastcgi_module 禁用ngx_http_fastcgi_module支持（该模块允许Nginx 与FastCGI 进程交互，并通过传递参数来控制FastCGI 进程工作。 ）FastCGI一个常驻型的公共网关接口。

--without-http_uwsgi_module 禁用ngx_http_uwsgi_module支持（该模块用来医用uwsgi协议，uWSGI服务器相关）

--without-http_scgi_module 禁用ngx_http_scgi_module支持（该模块用来启用SCGI协议支持，SCGI协议是CGI协议的替代。它是一种应用程序与HTTP服务接口标准。它有些像FastCGI但他的设计 更容易实现。）

--without-http_memcached_module 禁用ngx_http_memcached_module支持（该模块用来提供简单的缓存，以提高系统效率）

-without-http_limit_zone_module 禁用ngx_http_limit_zone_module支持（该模块可以针对条件，进行会话的并发连接数控制）

--without-http_limit_req_module 禁用ngx_http_limit_req_module支持（该模块允许你对于一个地址进行请求数量的限制用一个给定的session或一个特定的事件）

--without-http_empty_gif_module 禁用ngx_http_empty_gif_module支持（该模块在内存中常驻了一个1*1的透明GIF图像，可以被非常快速的调用）

--without-http_browser_module 禁用ngx_http_browser_module支持（该模块用来创建依赖于请求报头的值。如果浏览器为modern ，则$modern_browser等于modern_browser_value指令分配的值；如 果浏览器为old，则$ancient_browser等于 ancient_browser_value指令分配的值；如果浏览器为 MSIE中的任意版本，则 $msie等于1）

--without-http_upstream_ip_hash_module 禁用ngx_http_upstream_ip_hash_module支持（该模块用于简单的负载均衡）

--with-http_perl_module 启用ngx_http_perl_module支持（该模块使nginx可以直接使用perl或通过ssi调用perl）

--with-perl_modules_path= 设定perl模块路径

--with-perl= 设定perl库文件路径

--http-log-path= 设定access log路径

--http-client-body-temp-path= 设定http客户端请求临时文件路径

--http-proxy-temp-path= 设定http代理临时文件路径

--http-fastcgi-temp-path= 设定http fastcgi临时文件路径

--http-uwsgi-temp-path= 设定http uwsgi临时文件路径

--http-scgi-temp-path= 设定http scgi临时文件路径
-without-http 禁用http server功能

--without-http-cache 禁用http cache功能

--with-mail 启用POP3/IMAP4/SMTP代理模块支持

--with-mail_ssl_module 启用ngx_mail_ssl_module支持

--without-mail_pop3_module 禁用pop3协议（POP3即邮局协议的第3个版本,它是规定个人计算机如何连接到互联网上的邮件服务器进行收发邮件的协议。是因特网电子邮件的第一个离线协议标 准,POP3协议允许用户从服务器上把邮件存储到本地主机上,同时根据客户端的操作删除或保存在邮件服务器上的邮件。POP3协议是TCP/IP协议族中的一员，主要用于 支持使用客户端远程管理在服务器上的电子邮件）

--without-mail_imap_module 禁用imap协议（一种邮件获取协议。它的主要作用是邮件客户端可以通过这种协议从邮件服务器上获取邮件的信息，下载邮件等。IMAP协议运行在TCP/IP协议之上， 使用的端口是143。它与POP3协议的主要区别是用户可以不用把所有的邮件全部下载，可以通过客户端直接对服务器上的邮件进行操作。）

--without-mail_smtp_module 禁用smtp协议（SMTP即简单邮件传输协议,它是一组用于由源地址到目的地址传送邮件的规则，由它来控制信件的中转方式。SMTP协议属于TCP/IP协议族，它帮助每台计算机在发送或中转信件时找到下一个目的地。）

--with-google_perftools_module 启用ngx_google_perftools_module支持（调试用，剖析程序性能瓶颈）

--with-cpp_test_module 启用ngx_cpp_test_module支持

--add-module= 启用外部模块支持

--with-cc= 指向C编译器路径

--with-cpp= 指向C预处理路径

--with-cc-opt= 设置C编译器参数（PCRE库，需要指定–with-cc-opt=”-I /usr/local/include”，如果使用select()函数则需要同时增加文件描述符数量，可以通过–with-cc- opt=”-D FD_SETSIZE=2048”指定。）

--with-ld-opt= 设置连接文件参数。（PCRE库，需要指定–with-ld-opt=”-L /usr/local/lib”。）

--with-cpu-opt= 指定编译的CPU，可用的值为: pentium, pentiumpro, pentium3, pentium4, athlon, opteron, amd64, sparc32, sparc64, ppc64

--without-pcre 禁用pcre库

--with-pcre 启用pcre库

--with-pcre= 指向pcre库文件目录

--with-pcre-opt= 在编译时为pcre库设置附加参数

--with-md5= 指向md5库文件目录（消息摘要算法第五版，用以提供消息的完整性保护）

--with-md5-opt= 在编译时为md5库设置附加参数

--with-md5-asm 使用md5汇编源

--with-sha1= 指向sha1库目录（数字签名算法，主要用于数字签名）

--with-sha1-opt= 在编译时为sha1库设置附加参数

--with-sha1-asm 使用sha1汇编源

--with-zlib= 指向zlib库目录

--with-zlib-opt= 在编译时为zlib设置附加参数

--with-zlib-asm= 为指定的CPU使用zlib汇编源进行优化，CPU类型为pentium, pentiumpro

--with-libatomic 为原子内存的更新操作的实现提供一个架构

--with-libatomic= 指向libatomic_ops安装目录

--with-openssl= 指向openssl安装目录

--with-openssl-opt 在编译时为openssl设置附加参数

--with-debug 启用debug日志

转自：<http://blog.sina.com.cn/s/blog_68c25adf01014037.html>
站点：运维生存时间  网址：<http://www.ttlsa.com/html/3596.html>

### nginx安装配置+清缓存模块安装

经过一段时间的使用，发现nginx在并发与负载能力方面确实优于apache，现在已经将大部分站点从apache转到了nginx了。以下是nginx的一些简单的安装配置。
环境
操作系统：CentOS、RedHat
IP地址：192.168.1.202

**下载软件包**
```
# mkdir /usr/local/src/tarbag
# mkdir /usr/local/src/software
# cd /usr/local/src/tarbag/
```

Nginx
```
# wget <http://www.nginx.org/download/nginx-1.0.6.tar.gz>
```
Nginx cache purge模块(可选)
```
# wget <http://labs.frickle.com/files/ngx_cache_purge-1.3.tar.gz>
```

**编译安装**
```
# cd /usr/local/src/tarbag/
# tar -xzvf nginx-1.0.6.tar.gz -C /usr/local/src/software
# tar -xzvf ngx_cache_purge-1.3.tar.gz -C /usr/local/src/software
# cd /usr/local/src/software/
# ./configure \
--prefix=/usr/local/nginx-1.0.6 \  # 安装路径
--with-http_stub_status_module \ # 启用nginx状态模块
--with-http_ssl_module \ # 启用SSL模块
--with-http_realip_module \ # 启用realip模块（将用户IP转发给后端服务器）
--add-module=../ngx_cache_purge-1.3 # 添加缓存清除扩展模块
# make
# make install
```

**内核参数优化**
```
# vi sysctl.conf  增加以下配置
net.ipv4.netfilter.ip_conntrack_tcp_timeout_established = 1800
net.ipv4.ip_conntrack_max = 16777216　＃　如果使用默认参数,容易出现网络丢包
net.ipv4.netfilter.ip_conntrack_max = 16777216＃　如果使用默认参数,容易出现网络丢包
net.ipv4.tcp_max_syn_backlog = 65536
net.core.netdev_max_backlog =  32768
net.core.somaxconn = 32768
net.core.wmem_default = 8388608
net.core.rmem_default = 8388608
net.core.rmem_max = 16777216
net.core.wmem_max = 16777216
net.ipv4.tcp_timestamps = 0
net.ipv4.tcp_synack_retries = 2
net.ipv4.tcp_syn_retries =
net.ipv4.tcp_tw_recycle = 1
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_mem = 94500000 915000000 927000000
net.ipv4.tcp_max_orphans = 3276800
net.ipv4.ip_local_port_range = 1024  65535
配置生效
# sysctl –p
修改iptables启动脚本,在star()函数里面加上
# vi /etc/init.d/iptables
/sbin/sysctl  -p
```

**配置范例站点站点**

序号
域名
目录
1
www.heytool.com
/www/html/www.heytool.com
2
bbs.heytool.com
/www/html/bbs.heytool.com

**修改nginx配置文件：**

```
# vi nginx.conf
user  nobody nobody; # 运行nginx的所属组和所有者
worker_processes  2; # 开启两个nginx工作进程,一般几个CPU核心就写几
error_log  logs/error.log  notice; # 错误日志路径
pid        logs/nginx.pid; # pid路径
events {
worker_connections  1024; # 一个进程能同时处理1024个请求
}
http {
include       mime.types;
default_type  application/octet-stream;
 
log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
'$status $body_bytes_sent "$http_referer" '
'"$http_user_agent" "$http_x_forwarded_for"';
access_log  logs/access.log  main; # 默认访问日志路径
sendfile        on;
keepalive_timeout  65; # keepalive超市时间
# 开始配置一个域名,一个server配置段一般对应一个域名
server {
listen       80; #
# 在本机所有ip上监听80,也可以写为192.168.1.202:80,这样的话,就只监听192.168.1.202上的80口
server_name  www.heytool.com; # 域名
root   /www/html/www.heytool.com; # 站点根目录（程序目录）
index  index.html index.htm; # 索引文件
location / {  # 可以有多个location
root   /www/html/www.heytool.com; # 站点根目录（程序目录）
}
error_page   500 502 503 504  /50x.html;
# 定义错误页面,如果是500错误,则把站点根目录下的50x.html返回给用户
location = /50x.html {
root   /www/html/www.heytool.com;
}
}
# 开始配置站点bbs.heytool.com
server {
listen       80;
server_name  bbs.heytool.com;
root   /www/html/bbs.heytool.com;
index  index.html index.htm; # 索引文件
location / {
root   /www/html/bbs.heytool.com;
}
error_page   500 502 503 504  /50x.html;
location = /50x.html {
root   /www/html/bbs.heytool.com;
}
}
}
```
Nginx启动关闭
```
# /usr/local/nginx-1.0.6/sbin/nginx  //启动nginx
# /usr/local/nginx-1.0.6/sbin/nginx –t //测试nginx配置文件的准确性
# /usr/local/nginx-1.0.6/sbin/nginx –s reload //重载nginx
# /usr/local/nginx-1.0.6/sbin/nginx –s stop //关闭nginx
```

**测试**
创建测试站点
```
# mkdir –p /www/html/www.heytool.com
# mkdir –p /www/html/bbs.heytool.com
# echo “www.heytool.com” > /www/html/www.heytool.com/index.html
# echo “bbs.heytool.com” > /www/html/bbs.heytool.com/index.html
```

**绑定hosts,测试**

把两个域名指向192.168.1.202
192.168.1.202     www.heytool.com
192.168.1.202     bbs.heytool.com

完毕！！！！
更多配置参考：<http://www.docin.com/p-222277825.html>
参考视频教程：<http://www.kuaipan.cn/index.php?ac=file&oid=29331332391763978>

文章推荐：
[Haproxy+keepalived实现sphinx高可用负载均衡](http://who0168.blog.51cto.com/253401/693929)

[使用haproxy来实现sphinx负载均衡与健康监测](http://who0168.blog.51cto.com/253401/689962)

[持久化存储系统ttserver的安装配置](http://who0168.blog.51cto.com/253401/630097)


### nginx连接PHP 5.5

这两年IT更新换代的速度太快了,nginx从2年前的1.0版本到现在的1.5版本，各个版本共同开发。PHP从一开始的php 5.2到现在的5.3/5.4/5.5.起草这篇文章的时候发现官方已经停止了5.3的开发,最后的版本定格到了5.3.27,只会修复一些bug，宣告了5.3寿终正寝了.并且建议大家升级到5.4或者5.5, 鉴于版本更新得如此之快,决定写一片nginx连接php5.5的文章.于是乎,本草文写完了.

1. 安装PHP 5.5.0

- 下载
```
cd /usr/local/src/
wget http://www.php.net/get/php-5.5.0.tar.bz2/from/jp1.php.net/mirror
```

如果以上PHP不存在了,大家可以直接到官方下载. 如果还是找不到可以留言,我将会通过邮箱发送.

- 安装依赖包
确保安装之前有安装gd,png,curl,xml等等lib开发库。如果不确定，执行以下命令：
```
yum install gcc make gd-devel libjpeg-devel libpng-devel libxml2-devel bzip2-devel libcurl-devel -y
```

- 编译安装PHP 5.5.0
以下参数支持,ftp,图片函数,pdo等支持,因为使用了php自带的mysqlnd，所以不需要额外安装mysql的lib库了.如果你是64位系统，参数后面加上`--with-libdir=lib64`，如果不是可以跳过。

```
tar -xjf php-5.5.0.tar.bz2
cd php-5.5.0
./configure --prefix=/usr/local/php-5.5.0 --with-config-file-path=/usr/local/php-5.5.0/etc --with-bz2 --with-curl --enable-ftp --enable-sockets --disable-ipv6 --with-gd --with-jpeg-dir=/usr/local --with-png-dir=/usr/local --with-freetype-dir=/usr/local --enable-gd-native-ttf --with-iconv-dir=/usr/local --enable-mbstring --enable-calendar --with-gettext --with-libxml-dir=/usr/local --with-zlib --with-pdo-mysql=mysqlnd --with-mysqli=mysqlnd --with-mysql=mysqlnd --enable-dom --enable-xml --enable-fpm --with-libdir=lib64
make
make install
```

备注：如果PHP不需要curl和ftp的支持,可以将以上的`--with-curl --enable-ftp`去掉. 如果你是专业的linux从业人员,你完全可以看着help来选择你的安装参数，如果你不是的话,我建议你直接复制黏贴我的配置参数.这样可以少走一些弯路.

- 配置php
```
cp php.ini-production /usr/local/php-5.5.0/etc/php.ini
cp /usr/local/php-5.5.0/etc/php-fpm.conf.default /usr/local/php-5.5.0/etc/php-fpm.conf
```

- 启动php-fpm
```
/usr/local/php-5.5.0/sbin/php-fpm
```
执行以上命令，如果没报错一般情况下表示启动正常,如果不放心,也可以通过端口判断是PHP否启动
```
# netstat -lnt | grep 9000
tcp 0 0 127.0.0.1:9000 0.0.0.0:* LISTEN
```

2. 安装配置nginx

- 安装nginx
请看[ttlsa教程系列之nginx - nginx安装](http://bbs.linuxtone.org/thread-25588-1-1.html)
 
- 配置测试站点test.ttlsa.com

```
mkdir /data/logs/nginx/ # 用于存放nginx日志.请看2.3的配置文件
mkdir -p /data/site/test.ttlsa.com/ # 站点根目录
vim /data/site/test.ttlsa.com/info.php
<?php
phpinfo();
?>
```
保存退出

- nginx配置
在nginx.conf的http断中加上如下内容：
```
server {
listen 80;
server_name test.ttlsa.com;
access_log /data/logs/nginx/test.ttlsa.com.access.log main;
 
index index.php index.html index.html;
root /data/site/test.ttlsa.com;
 
location /
{
try_files $uri $uri/ /index.php?$args;
}
 
location ~ .*\.(php)?$
{
expires -1s;
try_files $uri =404;
fastcgi_split_path_info ^(.+\.php)(/.+)$;
include fastcgi_params;
fastcgi_param PATH_INFO $fastcgi_path_info;
fastcgi_index index.php;
fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
fastcgi_pass 127.0.0.1:9000;
 
}
}
```

- **配置讲解**
nginx将会连接回环地址9000端口执行PHP文件,需要使用tcp/ip协议,速度比较慢.建议大家换成使用socket方式连接。将fastcgi_pass 127.0.0.1:9000;改成fastcgi_pass unix:/var/run/phpfpm.sock;

- 启动nginx
```
/usr/local/nginx-1.4.1/sbin/nginx
```

3. 访问测试
```
# curl http://test.ttlsa.com/info.php
test php
```

出现如上内容,说明PHP安装完成。

### nginx配置虚拟主机

本节主要讲解如果使用nginx配置多个虚拟主机,也就是我们通常说的配置域名.接下来我们配置两个域名a.ttlsa.com，b.ttlsa.com。

#### 准备站点

我们站点统一放到/data/site下，每个站点根目录名称都和域名相同,具体如下。
新建a.ttlsa.com的站点根目录
```
# mkdir -p /data/site/a.ttlsa.com
```

- 新建a站的首页index.html
```
# cat /data/site/a.ttlsa.com/index.html
this is a.ttlsa.com!
```

- 新建b.ttlsa.com站点根目录
```
# mkdir -p /data/site/b.ttlsa.com
```

- 新建b站首页index.html,内容如this is b.ttlsa.com！
```
# cat /data/site/b.ttlsa.com/index.html
this is b.ttlsa.com!
```

- 新建日志文件目录
```
# mkdir -p /data/logs/nginx
```

我们统一讲日志存放到/data/logs下,这边是存放nginx日志,所以nginx日志保持在当前的nginx目录下.日志统一存放相对来说比较规范（如果你不习惯,你可以按自己的方式来做）

#### 配置nginx虚拟主机

- 增加nginx主配置文件nginx.conf
先配置nginx日志格式，在nginx.conf找到如下内容，并且将#注释标志去掉
```
#log_format main '$remote_addr - $remote_user [$time_local] "$request" '
# '$status $body_bytes_sent "$http_referer" '
# '"$http_user_agent" "$http_x_forwarded_for"';
```

- 配置nginx主配置文件
```
# vim /usr/local/nginx-1.5.1/conf/nginx.conf
server{
server_name a.ttlsa.com;
listen 80;
root /data/site/a.ttlsa.com;
 
access_log /data/logs/nginx/a.ttlsa.com-access.log main;
location /
{
 
}
}
 
server{
server_name b.ttlsa.com;
listen 80;
root /data/site/b.ttlsa.com;
 
access_log /data/logs/nginx/b.ttlsa.com-access.log main;
location /
{
 
}
}
```

- 配置讲解
server{}：配置虚拟主机必须有这个段。
server_name：虚拟主机的域名，可以写多个域名，类似于别名，比如说你可以配置成
server_name b.ttlsa.com c.ttlsa.com d.ttlsa.com，这样的话，访问任何一个域名，内容都是一样的
listen 80，监听ip和端口，这边仅仅只有端口，表示当前服务器所有ip的80端口，如果只想监听127.0.0.1的80，写法如下：
listen 127.0.0.1:80
root /data/site/b.ttlsa.com：站点根目录，你网站文件存放的地方。注：站点目录和域名尽量一样，养成一个好习惯
access_log /data/logs/nginx/b.ttlsa.com-access.log main：访问日志
location /{} 默认uri,location具体内容后续讲解,大家关注一下.

#### 重启并打开站点

nginx -t 检查nginx配置是否ok，命令如下：
```
# /usr/local/nginx-1.5.1/sbin/nginx -t
nginx: the configuration file /usr/local/nginx-1.5.1/conf/nginx.conf syntax is ok
nginx: configuration file /usr/local/nginx-1.5.1/conf/nginx.conf test is successful
```

如果看到以上两行ok和successful就表示配置问题，那接下来我们启动nginx
启动nginx
```
# /usr/local/nginx-1.5.1/sbin/nginx
```

访问a.ttlsa.com、b.ttlsa.com（我这边DNS已经解析到了192.168.1.201，在测试的情况下，我们可以通过版本hosts即可），绑定host方法如下：
讲如下内容增加到C:\Windows\System32\Drivers\etc\hosts
```
192.168.1.201 a.ttlsa.com
192.168.1.201 b.ttlsa.com
```

以上是windows绑定hosts方式，如下是linux方式
```
echo "192.168.1.201 a.ttlsa.com
192.168.1.201 b.ttlsa.com" >> /etc/hosts
```

使用浏览器访问这两个站点。我这边使用curl来访问。
```
[root@ns conf]# curl http://a.ttlsa.com
this is a.ttlsa.com! //a站点内容
[root@ns conf]# curl http://b.ttlsa.com
this is b.ttlsa.com! //b站点内容
```

#### 其他指令
- 关闭nginx
```
/usr/local/nginx-1.5.1/sbin/nginx -s stop
```

- 重启nginx
```
/usr/local/nginx-1.5.1/sbin/nginx -s reload //修改配置之后reload,实际上严格意义来说这不是
```

转载请注明出处：<http://www.ttlsa.com/html/1571.html>


### nginx location配置 

今天讲下location的用法，部分内容是直接从网络上摘取的，这边做了一个整理，为了便于理解和学习，我这边做了一些例子。

语法规则： location [=|~|~*|^~] /uri/ { … }
猜测：这个[=|~|~*|^~]的作用更像是后面正则表达式的修饰符，起到不同的匹配作用，好像还可以省略掉

= 表示精确匹配,这个优先级也是最高的

^~ 表示uri以某个常规字符串开头，理解为匹配 url路径即可。nginx不对url做编码，因此请求为/static/20%/aa，可以被规则^~ /static/ /aa匹配到（注意是空格）。

~  表示区分大小写的正则匹配

~* 表示不区分大小写的正则匹配(和上面的唯一区别就是大小写)

!~和!~*分别为区分大小写不匹配及不区分大小写不匹配的正则

/ 通用匹配，任何请求都会匹配到，默认匹配.

**下面讲讲这些语法的一些规则和优先级**
多个location配置的情况下匹配顺序为（参考资料而来，还未实际验证，试试就知道了，不必拘泥，仅供参考）：
优先级 = > ^~ >
首先匹配 =，其次匹配^~, 其次是按文件中顺序的正则匹配，最后是交给 / 通用匹配。当有匹配成功时候，停止匹配，按当前匹配规则处理请求。

例子，有如下匹配规则：

```
location / {
   echo "/"; //需要安装echo模块才行,这边大家可以改成各自的规则
}
location = / {
   echo "=/";
}
location = /nginx {
   echo "=/nginx";
}
location ~ \.(gif|jpg|png|js|css)$ {
   echo "small-gif/jpg/png";
}
location ~* \.png$ {
   echo "all-png";
}
location ^~ /static/ {
   echo "static";
}
```

以下是各种的访问情况
访问`http://a.ttlsa.com/`.因为/是完全匹配的
如下：
```
# curl http://a.ttlsa.com/
 =/
```

访问http://a.ttlsa.com/nginx,因为完全匹配了"=/nginx"

```
# curl http://a.ttlsa.com/nginx
=/nginx
```
访问`http://a.ttlsa.com/nginx`,从第一个开始尝试匹配,最后匹配到了`~* \.png$` .
```
# curl http://a.ttlsa.com/xxx/1111.PNG （注意,这是大写）
all-png
```

访问`http://a.ttlsa.com/static/1111.png`,虽然static放在最后面,但是因为有^的缘故,他是最匹配的.
```
# curl http://a.ttlsa.com/static/1111.png
static
```
好了，最后给出我们先上环境的静态文件的匹配规则
```
location ~* .*\.(js|css)?$
{
        expires 7d; //7天过期,后续讲解
        access_log off; //不保存日志
}
 
location ~* .*\.(png|jpg|gif|jpeg|bmp|ico)?$
{        
        expires 7d;
        access_log off;
}
 
location ~* .*\.(zip|rar|exe|msi|iso|gho|mp3|rmvb|mp4|wma|wmv|rm)?$
{
        deny all; //禁止这些文件下载，大家可以根据自己的环境来配置
}
```


### nginx root&alias文件路径配置

nginx指定文件路径有两种方式root和alias，这两者的用法区别，使用方法总结了下，方便大家在应用过程中，快速响应。root与alias主要区别在于nginx如何解释location后面的uri，这会使两者分别以不同的方式将请求映射到服务器文件上。

[root]
语法：root path
默认值：root html
配置段：http、server、location、if
[alias]
语法：alias path
配置段：location
实例：
```
location ~ ^/weblogs/ {
 root /data/weblogs/www.ttlsa.com;
 autoindex on;
 auth_basic            "Restricted";
 auth_basic_user_file  passwd/weblogs;
}
```

如果一个请求的URI是`/weblogs/httplogs/www.ttlsa.com-access.log`时，web服务器将会返回服务器上的/data/weblogs/www.ttlsa.com/weblogs/httplogs/www.ttlsa.com-access.log的文件。
[info]root会根据完整的URI请求来映射，也就是/path/uri。[/info]
因此，前面的请求映射为`path/weblogs/httplogs/www.ttlsa.com-access.log`。

```
location ^~ /binapp/ {  
 limit_conn limit 4;
 limit_rate 200k;
 internal;  
 alias /data/statics/bin/apps/;
}
```

alias会把location后面配置的路径丢弃掉，把当前匹配到的目录指向到指定的目录。如果一个请求的URI是/binapp/a.ttlsa.com/favicon时，web服务器将会返回服务器上的/data/statics/bin/apps/a.ttlsa.com/favicon.jgp的文件。

[warning]1. 使用alias时，目录名后面一定要加"/"。
2. alias可以指定任何名称。
3. alias在使用正则匹配时，必须捕捉要匹配的内容并在指定的内容处使用。
4. alias只能位于location块中。[/warning]
如需转载请注明出处：  <http://www.ttlsa.com/html/2907.html>

### ngx_http_core_module模块提供的变量

ngx_http_core_module模块在处理请求时，会有大量的变量，这些变量可以通过访问日志来记录下来，也可以用于其它nginx模块。在我们对请求做策略如改写等等都会使用到一些变量，顺便对ngx_http_core_module模块提供的变量总结了下，如下所示：
```

2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
参数名称 注释
$arg_PARAMETER HTTP 请求中某个参数的值，如/index.php?site=www.ttlsa.com，可以用$arg_site取得www.ttlsa.com这个值.
$args HTTP 请求中的完整参数。例如，在请求/index.php?width=400&height=200 中，$args表示字符串width=400&height=200.
$binary_remote_addr 二进制格式的客户端地址。例如：\x0A\xE0B\x0E
$body_bytes_sent 表示在向客户端发送的http响应中，包体部分的字节数
$content_length 表示客户端请求头部中的Content-Length 字段
$content_type 表示客户端请求头部中的Content-Type 字段
$cookie_COOKIE 表示在客户端请求头部中的cookie 字段
$document_root 表示当前请求所使用的root 配置项的值
$uri 表示当前请求的URI，不带任何参数
$document_uri 与$uri 含义相同
$request_uri 表示客户端发来的原始请求URI，带完整的参数。$uri和$document_uri未必是用户的原始请求，在内部重定向后可能是重定向后的URI，而$request_uri 永远不会改变，始终是客户端的原始URI.
$host 表示客户端请求头部中的Host字段。如果Host字段不存在，则以实际处理的server（虚拟主机）名称代替。如果Host字段中带有端口，如IP:PORT，那么$host是去掉端口的，它的值为IP。$host 是全小写的。这些特性与http_HEADER中的http_host不同，http_host只取出Host头部对应的值。 
$hostname 表示 Nginx所在机器的名称，与 gethostbyname调用返回的值相同 
$http_HEADER 表示当前 HTTP请求中相应头部的值。HEADER名称全小写。例如，示请求中 Host头部对应的值 用 $http_host表 
$sent_http_HEADER 表示返回客户端的 HTTP响应中相应头部的值。HEADER名称全小写。例如，用 $sent_ http_content_type表示响应中 Content-Type头部对应的值 
$is_args 表示请求中的 URI是否带参数，如果带参数，$is_args值为 ?，如果不带参数，则是空字符串 
$limit_rate 表示当前连接的限速是多少，0表示无限速 
$nginx_version 表示当前 Nginx的版本号 
$query_string 请求 URI中的参数，与 $args相同，然而 $query_string是只读的不会改变 
$remote_addr 表示客户端的地址 
$remote_port 表示客户端连接使用的端口 
$remote_user 表示使用 Auth Basic Module时定义的用户名 
$request_filename 表示用户请求中的 URI经过 root或 alias转换后的文件路径 
$request_body 表示 HTTP请求中的包体，该参数只在 proxy_pass或 fastcgi_pass中有意义 
$request_body_file 表示 HTTP请求中的包体存储的临时文件名 
$request_completion 当请求已经全部完成时，其值为 “ok”。若没有完成，就要返回客户端，则其值为空字符串；或者在断点续传等情况下使用 HTTP range访问的并不是文件的最后一块，那么其值也是空字符串。
$request_method 表示 HTTP请求的方法名，如 GET、PUT、POST等 
$scheme 表示 HTTP scheme，如在请求 https://nginx.com/中表示 https 
$server_addr 表示服务器地址 
$server_name 表示服务器名称 
$server_port 表示服务器端口 
$server_protocol 表示服务器向客户端发送响应的协议，如 HTTP/1.1或 HTTP/1.0
```

如需转载请注明出处：<http://www.ttlsa.com/html/2912.html>

### nginx日志配置

日志对于统计排错来说非常有利的。本文总结了nginx日志相关的配置如access_log、log_format、open_log_file_cache、log_not_found、log_subrequest、rewrite_log、error_log。
nginx有一个非常灵活的日志记录模式。每个级别的配置可以有各自独立的访问日志。日志格式通过log_format命令来定义。ngx_http_log_module是用来定义请求日志格式的。

**1. access_log指令**

语法: 
```
access_log path [format [buffer=size [flush=time]]];
access_log path format gzip[=level] [buffer=size] [flush=time];
access_log syslog:server=address[,parameter=value] [format];
access_log off;
```
默认值: access_log logs/access.log combined;

配置段: http, server, location, if in location, limit_except
gzip压缩等级。

buffer设置内存缓存区大小。

flush保存在缓存区中的最长时间。

不记录日志：access_log off;

使用默认combined格式记录日志：access_log logs/access.log 或 access_log logs/access.log combined;

**2. log_format指令**
```
语法: log_format name string …;

默认值: log_format combined “…”;

配置段: http
name表示格式名称，string表示等义的格式。log_format有一个默认的无需设置的combined日志格式，相当于apache的combined日志格式，如下所示：
```

```
log_format  combined  '$remote_addr - $remote_user  [$time_local]  '
                                   ' "$request"  $status  $body_bytes_sent  '
                                   ' "$http_referer"  "$http_user_agent" ';
```

如果nginx位于负载均衡器，squid，nginx反向代理之后，web服务器无法直接获取到客户端真实的IP地址了。 $remote_addr获取反向代理的IP地址。反向代理服务器在转发请求的http头信息中，可以增加X-Forwarded-For信息，用来记录 客户端IP地址和客户端请求的服务器地址。PS: 获取用户真实IP 参见<http://www.ttlsa.com/html/2235.html>如下所示：

```
log_format  porxy  '$http_x_forwarded_for - $remote_user  [$time_local]  '
                             ' "$request"  $status $body_bytes_sent '
                             ' "$http_referer"  "$http_user_agent" ';
```

日志格式允许包含的变量注释如下：
```
$remote_addr, $http_x_forwarded_for 记录客户端IP地址
$remote_user 记录客户端用户名称
$request 记录请求的URL和HTTP协议
$status 记录请求状态
$body_bytes_sent 发送给客户端的字节数，不包括响应头的大小； 该变量与Apache模块mod_log_config里的“%B”参数兼容。
$bytes_sent 发送给客户端的总字节数。
$connection 连接的序列号。
$connection_requests 当前通过一个连接获得的请求数量。
$msec 日志写入时间。单位为秒，精度是毫秒。
$pipe 如果请求是通过HTTP流水线(pipelined)发送，pipe值为“p”，否则为“.”。
$http_referer 记录从哪个页面链接访问过来的
$http_user_agent 记录客户端浏览器相关信息
$request_length 请求的长度（包括请求行，请求头和请求正文）。
$request_time 请求处理时间，单位为秒，精度毫秒； 从读入客户端的第一个字节开始，直到把最后一个字符发送给客户端后进行日志写入为止。
$time_iso8601 ISO8601标准格式下的本地时间。
$time_local 通用日志格式下的本地时间。
```

[warning]发送给客户端的响应头拥有“sent_http_”前缀。 比如$sent_http_content_range。[/warning]
实例如下：

```
http {
 log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                                        '"$status" $body_bytes_sent "$http_referer" '
                                        '"$http_user_agent" "$http_x_forwarded_for" '
                                        '"$gzip_ratio" $request_time $bytes_sent $request_length';
 
 log_format srcache_log '$remote_addr - $remote_user [$time_local] "$request" '
                                '"$status" $body_bytes_sent $request_time $bytes_sent $request_length '
                                '[$upstream_response_time] [$srcache_fetch_status] [$srcache_store_status] [$srcache_expire]';
 
 open_log_file_cache max=1000 inactive=60s;
 
 server {
 server_name ~^(www\.)?(.+)$;
 access_log logs/$2-access.log main;
 error_log logs/$2-error.log;
 
 location /srcache {
 access_log logs/access-srcache.log srcache_log;
 }
 }
}
```

**3. open_log_file_cache指令**
```
语法: open_log_file_cache max=N [inactive=time] [min_uses=N] [valid=time];
open_log_file_cache off;
默认值: open_log_file_cache off;
配置段: http, server, location
对于每一条日志记录，都将是先打开文件，再写入日志，然后关闭。可以使用open_log_file_cache来设置日志文件缓存(默认是off)，格式如下：
参数注释如下：
max:设置缓存中的最大文件描述符数量，如果缓存被占满，采用LRU算法将描述符关闭。
inactive:设置存活时间，默认是10s
min_uses:设置在inactive时间段内，日志文件最少使用多少次后，该日志文件描述符记入缓存中，默认是1次
valid:设置检查频率，默认60s
off：禁用缓存
```
实例如下：
```
open_log_file_cache max=1000 inactive=20s valid=1m min_uses=2;
```

**4. log_not_found指令**
```
语法: log_not_found on | off;
默认值: log_not_found on;
配置段: http, server, location
是否在error_log中记录不存在的错误。默认是。
```

**5. log_subrequest指令**
```
语法: log_subrequest on | off;
默认值: log_subrequest off;
配置段: http, server, location
是否在access_log中记录子请求的访问日志。默认不记录。
```

**6. rewrite_log指令**
```
由ngx_http_rewrite_module模块提供的。用来记录重写日志的。对于调试重写规则建议开启。 Nginx重写规则指南
语法: rewrite_log on | off;
默认值: rewrite_log off;
配置段: http, server, location, if
启用时将在error log中记录notice级别的重写日志。
```

**7. error_log指令**
```
语法: error_log file | stderr | syslog:server=address[,parameter=value] [debug | info | notice | warn | error | crit | alert | emerg];
默认值: error_log logs/error.log error;
配置段: main, http, server, location
配置错误日志。
```

### apache和nginx支持SSI配置

**一. 前言**

SSI是一种类似于ASP的基于服务器的网页制作技术。将内容发送到浏览器之前，可以使用“服务器端包含 (SSI）”指令将文本、图形或应用程序信息包含到网页中。例如，可以使用 SSI 包含时间/日期戳、版权声明或供客户填写并返回的表单。对于在多个文件中重复出现的文本或图形，使用包含文件是一种简便的方法。将内容存入一个包含文件中即可，而不必将内容输入所有文件。通过一个非常简单的语句即可调用包含文件，此语句指示 Web 服务器将内容插入适当网页。而且，使用包含文件时，对内容的所有更改只需在一个地方就能完成。
因为包含 SSI 指令的文件要求特殊处理，所以必须为所有 SSI 文件赋予 SSI文件扩展名。默认扩展名是 .stm、.shtm 和 .shtml

**二. apache配置**

apache默认不支持ssi的，可以在apache下做如下设置：
修改Apache配置文件httpd.conf
1. 确认加载include.so模块，将注释去掉：

```
LoadModule include_module libexec/apache2/mod_include.so
```

2. AddType部分去掉这两段注释：

```
AddType text/html .shtml
AddOutputFilter INCLUDES .shtml
```

3. Directory目录权限里面找到
```
Options Indexes FollowSymLinks
增加Includes修改为：
Options Indexes FollowSymLinks Includes
```

4. 重新启动Apache

**三. nginx配置**

1. 相关指令说明：

ssi
```
语法：ssi [ on | off ]
默认值：ssi off
配置段段：http, server, location, if
启用SSI处理。
[warning]注意如果启用SSI，那么Last-Modified头和Content-Length头不会传递。[/warning]
ssi_silent_errors
语法：ssi_silent_errors [on|off]
默认值：ssi_silent_errors off
配置段：http, server, location
```
如果在处理SSI的过程中出现“[an error occurred while processing the directive]”错误，禁止将其输出。

ssi_types
```
语法：ssi_types mime-type [mime-type …]
默认值：ssi_types text/html
配置段：http, server, location
默认只解析text/html类型，这个参数可以指定其他的MIME类型。
ssi_value_length
语法：ssi_value_length length
默认值：ssi_value_length 256
配置段：http, server, location
```
定义允许SSI使用的参数值的长度。

在http段添加
```
ssi on;
ssi_silent_errors off;
ssi_types text/shtml;
```
或
```
location ~* \.shtml${
    ssi on;
    ssi_silent_errors off;
    ssi_value_length 1024;
    ssi_types text/shtml;
}
```
或
```
location / {
    ssi on;
    ssi_silent_errors on;
    ssi_value_length 1024;
    ssi_types text/shtml;
}
```

### nginx日志切割

nginx日志默认情况下统统写入到一个文件中，文件会变的越来越大，非常不方便查看分析。以日期来作为日志的切割是比较好的，通常我们是以每日来做统计的。下面来说说nginx日志切割。
关于nginx相关日志配置参见：《nginx日志配置》一文。logrotate用法参见《logrotate日志管理工具》。

**1. 定义日志轮滚策略**

\# vim nginx-log-rotate
```
/data/weblogs/*.log {
    nocompress
    daily
    copytruncate
    create
    notifempty
    rotate 7
    olddir /data/weblogs/old_log
    missingok
    dateext
    postrotate
        /bin/kill -HUP `cat /var/run/nginx.pid 2> /dev/null` 2> /dev/null || true
    endscript
}
```

[warning]/data/weblogs/*.log使用通配符时，/data/weblogs/目录下的所有匹配到的日志文件都将切割。如果要切割特定日志文件，就指定到该文件。[/warning]

**2. 设置计划任务**

\# vim /etc/crontab
```
59 23 * * * root ( /usr/sbin/logrotate -f /PATH/TO/nginx-log-rotate)
```
这样每天23点59分钟执行日志切割。
如需转载请注明出处： <http://www.ttlsa.com/html/3166.html>

### Nginx重写规则指南

当运维遇到要重写情况时，往往是要程序员把重写规则写好后，发给你，你再到生产环境下配置。对于重写规则说到底就是正则匹配，做运维的岂能对正则表达式不了解的？最起码最基本的正则表达式会写。套用一句阿里的话(某网友说是阿里说的，不清楚到底是不是出自阿里)“不懂程序的运维，不是好运维；不懂运维的开发，不是好开发。”。 正则表达式也是一门语言哈。当你学习一门语言时，必然会遇到该门语言的正则表达式这章节的。 在这里推荐一本非常好的正则表达式书，包含常用的语言的正则写法如sed、perl、bash、awk、php、c#、java、javascript、python、ruby等等，《Regular Expressions Cookbook, 2nd Edition》，也有中文版的，大家可以到网络上找找。
介绍nginx的重写模块，创建重写规则向导，便于快捷正确的创建新的重写规则，不求救于人。同时，如果想把apache转换成nginx，重写规则也是要改的咯。

**一. rewrite模块介绍**

nginx的重写模块是一个简单的正则表达式匹配与一个虚拟堆叠机结合。依赖于PCRE库，因此需要安装pcre。根据相关变量重定向和选择不同的配置，从一个location跳转到另一个location，不过这样的循环最多可以执行10次，超过后nginx将返回500错误。同时，重写模块包含set指令，来创建新的变量并设其值，这在有些情景下非常有用的，如记录条件标识、传递参数到其他location、记录做了什么等等。

**二. rewrite模块指令**

break
```
语法：break
默认值：none
使用字段：server, location, if
完成当前设置的重写规则，停止执行其他的重写规则。
```

last
```
如果有last参数，那么停止处理任何rewrite相关的指令，立即用替换后的新URI开始下一轮的location匹配

last的break的相同点在于，立即停止执行所有当前上下文的rewrite模块指令；不同点在于last参数接着用新的URI马上搜寻新的location，而break不会搜寻新的location，直接用这个新的URI来处理请求，这样能避免重复rewite。因此，在server上下文中使用last，而在location上下文中使用break。

参考资料：https://nigelzeng.iteye.com/blog/1731317
1、rewrite后接break指令，完成rewrite之后会执行完当前的location（或者是if）指令里的其他内容（停止执行当前这一轮的ngx_http_rewrite_module指令集），然后不进行新URL的重新匹配。

2、rewrite后接last指令，在完成rewrite之后停止执行当前这一轮的ngx_http_rewrite_module指令集已经后续的指令，进而为新的URL寻找location匹配。
```

if
```
语法：if (condition) { … }
默认值：none
使用字段：server, location
注意：尽量考虑使用trp_files代替。
判断的条件可以有以下值：
1. 一个变量的名称：空字符传”“或者一些“0”开始的字符串为false。
2. 字符串比较：使用=或!=运算符
3. 正则表达式匹配：使用~(区分大小写)和~*(不区分大小写)，取反运算!~和!~*。
4. 文件是否存在：使用-f和!-f操作符
5. 目录是否存在：使用-d和!-d操作符
7. 文件、目录、符号链接是否存在：使用-e和!-e操作符
8. 文件是否可执行：使用-x和!-x操作符
return
语法：return code
默认值：none
使用字段：server, location, if
停止处理并为客户端返回状态码。非标准的444状态码将关闭连接，不发送任何响应头。可以使用的状态码有：204，400，402-406，408，410, 411, 413, 416与500-504。如果状态码附带文字段落，该文本将被放置在响应主体。相反，如果状态码后面是一个URL，该URL将成为location头补值。没有状态码的URL将被视为一个302状态码。
```

rewrite
```
语法：rewrite regex replacement flag
默认值：none
使用字段：server, location, if
按照相关的正则表达式与字符串修改URI，指令按照在配置文件中出现的顺序执行。可以在重写指令后面添加标记。
注意：如果替换的字符串以http://开头，请求将被重定向，并且不再执行多余的rewrite指令。
尾部的标记(flag)可以是以下的值：
last - 停止处理重写模块指令，之后搜索location与更改后的URI匹配。
break - 完成重写指令。
redirect - 返回302临时重定向，如果替换字段用http://开头则被使用。
permanent - 返回301永久重定向。
rewrite_log
语法：rewrite_log on | off
默认值：rewrite_log off
使用字段：server, location, if
变量：无
```
启用时将在error log中记录notice级别的重写日志。

set
```
语法：set variable value
默认值：none
使用字段：server, location, if
为给定的变量设置一个特定值。
uninitialized_variable_warn
语法：uninitialized_variable_warn on|off
默认值：uninitialized_variable_warn on
使用字段：http, server, location, if
控制是否记录未初始化变量的警告信息。
```

**三. 重写规则组成部分**

3.1 任何重写规则的第一部分都是一个正则表达式
可以使用括号来捕获，后续可以根据位置来将其引用，位置变量值取决于捕获正则表达式中的顺序，$1引用第一个括号中的值，$2引用第二个括号中的值，以此类推。如：

```
^/images/([a-z]{2})/([a-z0-9]{5})/(.*)\.(png|jpg|gif)$
```

$1是两个小写字母组成的字符串，$2是由小写字母和0到9的数字组成的5个字符的字符串，$3将是个文件名，$4是png、jpg、gif中的其中一个。
3.2 重写规则的第二部分是URI
请求被改写。该URI可能包含正则表达式中的捕获的位置参数或这个级别下的nginx任何配置变量。如：
```
/data?file=$3.$4
```

如果这个URI不匹配nginx配置的任何location，那么将给客户端返回301(永久重定向)或302(临时重定向)的状态码来表示重定向类型。该状态码可以通过第三个参数来明确指定。
3.3 重写规则的第三部分
第三部分也就是尾部的标记(flag)。 last标记将导致重写后的URI搜索匹配nginx的其他location，最多可循环10次。如：
```
rewrite '^/images/([a-z]{2})/([a-z0-9]{5})/(.*)\.(png|jpg|gif)$' /data?file=$3.$4 last;
```

break指令可以当做自身指令。如：
```
if ($bwhog) {
 limit_rate 300k;
 break;
}
```

另一个停止重写模块处理指令是return， 来控制主HTTP模块处理请求。 这意味着，nginx直接返回信息给客户端，与error_page结合为客户端呈现格式化的HTML页面或激活不同的模块来完成请求。如果状态码附带文字段落，该文本将被放置在响应主体。相反，如果状态码后面是一个URL，该URL将成为location头补值。没有状态码的URL将被视为一个302状态码。如：
```
location = /image404.html {
 return 404 "image not found\n";
}
```

四. 实例
```
http {
 # 定义image日志格式
 log_format imagelog '[$time_local] ' $image_file ' ' $image_type ' ' $body_bytes_sent ' ' $status;
 # 开启重写日志
 rewrite_log on;
 
 server {
 root /home/www;
 
 location / {
 # 重写规则信息
 error_log logs/rewrite.log notice;  
 # 注意这里要用‘’单引号引起来，避免{}
 rewrite '^/images/([a-z]{2})/([a-z0-9]{5})/(.*)\.(png|jpg|gif)$' /data?file=$3.$4;
 # 注意不能在上面这条规则后面加上“last”参数，否则下面的set指令不会执行
 set $image_file $3;
 set $image_type $4;
 }
 
 location /data {
 # 指定针对图片的日志格式，来分析图片类型和大小
 access_log logs/images.log mian;
 root /data/images;
 # 应用前面定义的变量。判断首先文件在不在，不在再判断目录在不在，如果还不在就跳转到最后一个url里
 try_files /$arg_file /image404.html;
 }
 location = /image404.html {
 # 图片不存在返回特定的信息
 return 404 "image not found\n";
 }
}
```

**五. 创建新的重新规则**

在接到要创建新的重写规则时，要弄清楚需求是什么样的，再决定怎么做。毕竟重写也是耗资源的有效率之分的。 下面的这些问题有些帮助的：
1. 你的URL的模式是什么样的?
2. 是否有一个以上的方法来实现？
3. 是否需要捕获URL部分作为变量？
4. 重定向到另一个web上可以看到我的规则？
5. 是否要替换查询的字符串参数？
检查网站或应用程序布局，清楚URL模式。啰嗦一句：我一而再再而三的强调，运维不能与开发脱节，运维要参与到开发当中。如果有不止一种方法实现，创建一个永久重定向。同时，定义一个重写规范，来使网址清洁，还可以帮助网站更容易被找到。
实例1. 要将home目录重定向到主页面上，目录结构如下：

```
/
/home
/home/
/home/index
/home/index/
/index
/index.php
/index.php/
```

重写规则如下：
```
rewrite ^/(home(/index)?|index(\.php)?)/?$ $scheme://$host/ permanent;
```
指定$scheme和$host变量，因为要做一个永久重定向并希望nginx使用相同的参数来构造URL。
实例2. 如果想分别记录各个部分的URL，可以使用正则表达式来捕获URI，然后，给变量分配指定位置变量，见上面的实例。
实例3. 当重写规则导致内部重定向或指示客户端调用该规则本身被定义的location时，必须采取特殊的动作来避免重写循环。如：在server配置段定义了一条规则带上last标志，在引用location时，必须使用break标志。

```
server {
 rewrite ^(/images)/(.*)\.(png|jpg|gif)$ $1/$3/$2.$3 last;
 location /images/ {
 rewrite ^(/images)/(.*)\.(png|jpg|gif)$ $1/$3/$2.$3 break;
 }
}
```

实例4. 作为重写规则的一部分，传递新的查询字符串参数是使用重写规则的目标之一。 如：
```
rewrite ^/images/(.*)_(\d+)x(\d+)\.(png|jpg|gif)$ /resizer/$1.$4?width=$2&height=$3? last;
```

nginx重写规则说起来挺简单的，做起来就难，重点在于正则表达式，同时，还需要考虑到nginx执行顺序。有疑问的朋友，欢迎加入ttlsa群，一起讨论，联系方式在网站中有，也可以跟帖说明。

### nginx 逻辑运算

nginx的配置中不支持if条件的逻辑与&& 逻辑或|| 运算 ，而且不支持if的嵌套语法，否则会报下面的错误：nginx: [emerg] invalid condition。
我们可以用变量的方式来间接实现。
要实现的语句：

```
if ($arg_unitid = 42012 && $uri ~/thumb/){
 echo "www.ttlsa.com";
}
```

如果按照这样来配置，就会报nginx: [emerg] invalid condition错误。
可以这么来实现，如下所示：
```
set $flag 0;
if ($uri ~ ^/thumb/[0-9]+_160.jpg$){
 set $flag "${flag}1";
}
if ($arg_unitid = 42012){
 set $flag "${flag}1";
}
if ($flag = "011"){
 echo "www.ttlsa.com";
}
```

转载请注明来自运维生存时间:  <http://www.ttlsa.com/html/3876.html>

### 隐藏Nginx版本号的安全性与方法

搭建好nginx或者apache，为了安全起见我们都会隐藏他们的版本号，这边讲的是nginx的版本号，如果你也想隐藏apache的版本号，那请点前面的链接。请看nginx版本号信息隐藏文章。
Nginx默认是显示版本号的，如：
```
[root@bkjz ~]# curl -I www.nginx.org
```

```
HTTP/1.1 200 OK
Server: nginx/0.8.44
Date: Tue, 13 Jul 2010 14:05:11 GMT
Content-Type: text/html
Content-Length: 8284
Last-Modified: Tue, 13 Jul 2010 12:00:13 GMT
Connection: keep-alive
Keep-Alive: timeout=15
Accept-Ranges: bytes
```

这样就给人家看到你的服务器nginx版本是0.8.44，前些时间暴出了一些Nginx版本漏洞，就是说有些版本有漏洞，而有些版本没有。这样暴露出来的版本号就容易变成攻击者可利用的信息。所以，从安全的角度来说，隐藏版本号会相对安全些！
那nginx版本号可以隐藏不？其实可以的，看下面的步骤：

1、进入nginx配置文件的目录（此目录根据安装时决定），用vim编辑打开
```
# vim nginx.conf
```

在http {—}里加上server_tokens off; 如：
```
http {
……省略
sendfile on;
tcp_nopush on;
keepalive_timeout 60;
tcp_nodelay on;
server_tokens off;
…….省略
}
```

2、编辑php-fpm配置文件，如fastcgi.conf或fcgi.conf（这个配置文件名也可以自定义的，根据具体文件名修改）：

```
找到：
fastcgi_param SERVER_SOFTWARE nginx/$nginx_version;
改为：
fastcgi_param SERVER_SOFTWARE nginx;
```

3、重新加载nginx配置：
```
# /etc/init.d/nginx reload
```

这样就完全对外隐藏了nginx版本号了，就是出现404、501等页面也不会显示nginx版本。
下面测试一下：
```
# curl -I www.abc.net
HTTP/1.1 200 OK
Server: nginx
Date: Tue, 13 Jul 2010 14:26:56 GMT
Content-Type: text/html; charset=UTF-8
Connection: keep-alive
Vary: Accept-Encoding
```


### CDN调度器HAProxy、Nginx、Varnish

CDN功能如下：
1、将全网IP分为若干个IP段组，分组的依据通常是运营商或者地域，目的是让相同网络环境中的用户聚集到相同的组内；
2、依据CDN服务器们的网络和容量，确定哪些CDN服务器适合服务哪些IP段组；
3、根据以上两步得到的结论，让用户去最适合他的服务器得到服务。
说白了，就是根据用户不同的来源IP把用户请求重定向到不同的CDN服务器上去。
那么，如何实现呢？
智能DNS是办法之一，稳定可靠且有效。
但至少在两个环境下它不能完全满足我们：
1、需要特别精细的调度时。由于大多数DNS Server不支持DNS扩展协议，所以拿不到用户的真实IP，只能根据Local DNS来调度。
2、访问特别频繁时。由于每次调度都将触发一次DNS，如果请求变得密集，DNS请求本身带来的开销也会相应变大；
3、需要根据服务器的带宽容量、连接数、负载情况、当机与否来调度时。由于DNS Server没有CDN节点服务器的信息，这种调度会变得困难。
这时候我们可以：
1、将用户先行引导到某一台或几台统一的服务器上去；
2、让它拿到用户的真实IP，计算出服务他的服务器；
3、通过HTTP302或其它方式把用户定位到最终服务器上。
部署在用户先访问到的那几台服务器上，负责定位IP然后重定向用户请求的那个软件，我们叫它“调度器”。
HAProxy实现：
HAProxy不支持形如0.0.0.1-0.8.255.255 cn的IP段表示方法，只支持1.1.4.0/22 “CN”的IP段表示方法。
1、我们需要先把IP段转化成它认识的方式；
a> 下载iprang.c或者iprang.c本地镜像；
b> 编译gcc -s -O3 -o iprange iprange.c；
c> 整理IP段列表geo.txt形如：

```
# head geo.txt
"1.0.0.0","1.0.0.255","AU"
"1.0.1.0","1.0.3.255","CN"
"1.0.4.0","1.0.7.255","AU"
"1.0.8.0","1.0.15.255","CN"
"1.0.16.0","1.0.31.255","JP"
"1.0.32.0","1.0.63.255","CN"
"1.0.64.0","1.0.127.255","JP"
"1.0.128.0","1.0.255.255","TH"
"1.1.0.0","1.1.0.255","CN"
"1.1.1.0","1.1.1.255","AU"
```


d> 输出HAProxy认识的IP段列表：
```
# cut -d, -f1,2,5 geo.txt | ./iprange | head
1.0.0.0/24 "AU"
1.0.1.0/24 "CN"
1.0.2.0/23 "CN"
1.0.4.0/22 "AU"
1.0.8.0/21 "CN"
1.0.16.0/20 "JP"
1.0.32.0/19 "CN"
1.0.64.0/18 "JP"
1.0.128.0/17 "TH"
1.1.0.0/24 "CN"
1.1.1.0/24 "AU"
```

e> 便于管理的目的，将整合后的IP段归类到同一个文件中：
```
# cut -d, -f1,2,5 geo.txt | ./iprange | sed 's/"//g' | awk -F' ' '{ print $1 >> $2".subnets" }'
# ls *.subnets
A1.subnets  AX.subnets  BW.subnets  CX.subnets  FJ.subnets  GR.subnets  IR.subnets  LA.subnets  ML.subnets  NF.subnets  PR.subnets  SI.subnets  TK.subnets  VE.subnets
# cat AU.subnets 
1.0.0.0/24
1.0.4.0/22
1.1.1.0/24
```

f> 把这些文件放到同一个文件夹下，我们以/etc/haproxy/conf/为例。
2、正确配置HAProxy以这些IP段为规则正确调度；
下面是一个haproxy.cfg的例子。配置好后重启Haproxy即可。

```
global
    log         127.0.0.1 local2 debug
 
    chroot      /var/lib/haproxy
    pidfile     /var/run/haproxy.pid
    maxconn     8000
    user        haproxy
    group       haproxy
    daemon
 
    stats socket /var/lib/haproxy/stats
 
defaults
    mode                    http
    log                     global
    option                  httplog
    option                  dontlognull
    option http-server-close
    option forwardfor       except 127.0.0.0/8
    option                  redispatch
    retries                 3
    timeout http-request    10s
    timeout queue           1m
    timeout connect         10s
    timeout client          1m
    timeout server          1m
    timeout http-keep-alive 10s
    timeout check           10s
    maxconn                 8000
 
frontend  main *:5000
    acl geo_A1 src -f /etc/haproxy/conf/A1.subnets
    acl geo_AX src -f /etc/haproxy/conf/AX.subnets
    acl geo_BW src -f /etc/haproxy/conf/BW.subnets
    acl geo_CX src -f /etc/haproxy/conf/CX.subnets
    acl geo_FJ src -f /etc/haproxy/conf/FJ.subnets
 
    ...
 
    reqrep ^([^\ ]*)\ /(.*)\ HTTP    \1\ /\2&ipfrom=A1\ HTTP if geo_A1
    reqrep ^([^\ ]*)\ /(.*)\ HTTP    \1\ /\2&ipfrom=AX\ HTTP if geo_AX
    reqrep ^([^\ ]*)\ /(.*)\ HTTP    \1\ /\2&ipfrom=BW\ HTTP if geo_BW
    reqrep ^([^\ ]*)\ /(.*)\ HTTP    \1\ /\2&ipfrom=CX\ HTTP if geo_CX
    reqrep ^([^\ ]*)\ /(.*)\ HTTP    \1\ /\2&ipfrom=FJ\ HTTP if geo_FJ
 
    ...
 
    default_backend             static
 
backend static
    server      static 127.0.0.1:6081 check
```

Nginx实现：
Nginx可以在核心模块HttpGeoModule（<http://wiki.nginx.org/HttpGeoModule>）的配合下实现调度：

```
http{
 
...
 
geo $useriprang {
    ranges;
    default a;
    0.0.0.1-0.8.255.255 a;
    0.9.0.0-0.255.255.255   a;
    1.0.0.0-1.0.0.255   a;
    1.0.1.0-1.0.1.255   b;
    1.0.2.0-1.0.3.255   b;
    1.0.4.0-1.0.7.255   a;
    ...
    223.255.252.0-223.255.253.255   c;
    223.255.254.0-223.255.254.255   a;
    223.255.255.0-223.255.255.255   a;
}
 
upstream backend {
    server 127.0.0.1:81;
}
 
server {
    listen       80;
    client_max_body_size 10240m;
 
    location / {
        proxy_redirect off;
        proxy_pass http://backend$request_uri&useriprang=$useriprang;
        proxy_next_upstream http_502 http_504 error timeout invalid_header;
        proxy_cache cache_one;
        proxy_cache_key $host:$server_port$uri$is_args$args;
        expires  5s;
    }
 
}
 
...
 
}
```

Varnish实现：
Varnish则有两个插件可以实现调度：
<https://github.com/cosimo/varnish-geoip> （Last updated: 28/05/2013）
<https://github.com/meetup/varnish-geoip-plugin> （Last updated: 2010）
性能问题
如上所述，使用Haproxy、Nginx、Varnish都能快速实现这个功能。
其中Nginx和Varnish使用了二分法在IP表中定位用户IP，而Haproxy是逐条过滤。
所以在IP分得较细，IP段组较多（归类后超过1000组）时，Haproxy会出现明显的性能衰减，其余两者没有这个问题。
其它
本文使用的软件版本如下：
HAProxy1.4.22，Nginx1.2.9，Varnish3.0.4。
HAProxy和Varnish都是目前的最新版本。
本文有参考http://blog.exceliance.fr/2012/07/02/use-geoip-database-within-haproxy/
转自：<http://blog.yikuyiku.com/?p=3851>


### lnmp架构下php安全配置分享


以往的lamp网站向着lnmp发展, 笔者工作环境使用lnmp多年, 在这里很高兴和大家分享一下多年的lnmp网站的php安全配置，至于lamp安全后续与大家分享，其实内容上八成相同，这边着重讲php安全配置，看内容.
**1. 使用open_basedir限制虚拟主机跨目录访问**
```
[HOST=www.ttlsa.com]
open_basedir=/data/site/www.ttlsa.com/:/tmp/
 
[HOST=test.ttlsa.com]
open_basedir=/data/site/test.ttlsa.com/:/tmp/
```

如上配置的意思是www.ttlsa.com下的php程序被限制在open_basedir配置的两个目录下, 不可以访问到其他目录。如果没有做以上的配置，那么test.ttlsa.com与www.ttlsa.com的程序可以互相访问.
如果其中一个站点有漏洞被黑客植入了webshell，那么他可以通过这个站点拿下同一台服务器的其他站点，最后挂木马.
[warning]注意：目录最后一定要加上/. 比如你写/tmp，你的站点同时存在/tmp123等等以/tmp开头的目录，那么黑客也可以访问到这些目录，另外, php5.3以上支持这个写法，5.2不支持。[/warning]

**2. 禁用不安全PHP函数**

```
disable_functions = show_source,system,shell_exec,passthru,exec,popen,proc_open,proc_get_status,phpinfo
```
禁止php执行以上php函数,以上php程序可以执行linux命令, 比如可以执行ping、netstat、mysql等等.如果你的系统有提权bug,后果你懂得.

**3. 关注软件安全资讯**

积极关注linux内核、php安全等信息并及时采取错误

**4. php用户只读**

这个方法是我最推崇的方法，但是执行之前一定要和php工程师商量. 为什么？例如站点www.ttlsa.com根目录用户与组为nobody，而运行php的用户和组为phpuser。目录权限为755，文件权限为644. 如此，php为只读，无法写入任何文件到站点目录下。也就是说用户不能上传文件，即使有漏洞, 黑客也传不了后门, 更不可能挂木马.  这么干之前告知程序员将文件缓存改为nosql内存缓存（例如memcached、redis等），上传的文件通过接口传到其他服务器（静态服务器）。
[warning]备注：程序生成本地缓存是个非常糟糕的习惯，使用文件缓存速度缓慢、浪费磁盘空间、最重要一点是一般情况下服务器无法横向扩展.[/warning]

**5. 关闭php错误日志**
```
display_errors = On
```
改为
```
display_errors = Off
```

程序一旦出现错误，详细错误信息便立刻展示到用户眼前，其中包含路径、有的甚至是数据库账号密码. 注入渗透密码基本上都是通过这个报错来猜取。生产环境上强烈关闭它

**6. php上传分离**


将文件上传到远程服务器，例如nfs等。当然也可以调用你们写好的php接口. 即使有上传漏洞，那么文件也被传到了静态服务器上。木马等文件根本无法执行.
举个例子：
php站点www.ttlsa.com，目录/data/site/www.ttlsa.com
静态文件站点static.ttlsa.com，目录/data/site/static.ttlsa.com
文件直接被传到了/data/site/static.ttlsa.com，上传的文件无法通过www.ttlsa.com来访问，只能使用static.ttlsa.com访问，但是static.ttlsa.com不支持php.

**7. 关闭php信息**
```
expose_php = On
```
改为
```
expose_php = Off
```

不轻易透露自己php版本信息，防止黑客针对这个版本的php发动攻击.

**8. 禁止动态加载链接库**
```
disable_dl = On;
改为
enable_dl = Off;
```

**9. 禁用打开远程url**
```
allow_url_fopen = On
改为
allow_url_fopen = Off
```

其实这点算不上真正的安全, 并不会导致web被入侵等问题,但是这个非常影响性能, 笔者认为它属于狭义的安全问题.
以下方法将无法获取远程url内容
```
$data = file_get_contents("http://www.baidu.com/")；
```

以下方法可以获取本地文件内容
```
$data = file_get_contents("1.txt");
```
如果你的站点访问量不大、数据库也运行良好，但是web服务器负载出奇的高，请你直接检查下是否有这个方法。笔者遇到过太多这个问题，目前生产环境已全线禁用，如果php工程师需要获取远程web的内容，建议他们使用curl.

php curl如何使用请查看我之前的文章《PHP使用curl替代file_get_contents》，以及php下curl与file_get_contents性能对比.

### nginx tcp代理

nginx tcp代理功能由nginx_tcp_proxy_module模块提供，同时监测后端主机状态。该模块包括的模块有： ngx_tcp_module, ngx_tcp_core_module, ngx_tcp_upstream_module, ngx_tcp_proxy_module, ngx_tcp_upstream_ip_hash_module。

**1. 安装**
```
# wget http://nginx.org/download/nginx-1.4.4.tar.gz
# tar zxvf nginx-1.4.4.tar.gz
# cd nginx-1.4.4
# ./configure --add-module=/path/to/nginx_tcp_proxy_module
# make
# make install
```

**2. 配置**
```
http {
    listen 80;
    location /status {
        check_status;
    }
}
tcp {
    upstream cluster_www_ttlsa_com {
        # simple round-robin
        server 127.0.0.1:1234;
        check interval=3000 rise=2 fall=5 timeout=1000;
        #check interval=3000 rise=2 fall=5 timeout=1000 type=ssl_hello;
        #check interval=3000 rise=2 fall=5 timeout=1000 type=http;
        #check_http_send "GET / HTTP/1.0\r\n\r\n";
        #check_http_expect_alive http_2xx http_3xx;
    }
    server {
        listen 8888;
        proxy_pass cluster_www_ttlsa_com;
    }
}
```
这会出现一个问题，就是tcp连接会掉线。原因在于当服务端关闭连接的时候，客户端不可能立刻发觉连接已经被关闭，需要等到当Nginx在执行check规则时认为服务端链接关闭，此时nginx会关闭与客户端的连接。

**3. 保持连接配置**
```
http {
    listen 80;
    location /status {
        check_status;
    }
}
tcp {
 timeout 1d;
    proxy_read_timeout 10d;
    proxy_send_timeout 10d;
    proxy_connect_timeout 30;
    upstream cluster_www_ttlsa_com {
        # simple round-robin
        server 127.0.0.1:1234;
        check interval=3000 rise=2 fall=5 timeout=1000;
        #check interval=3000 rise=2 fall=5 timeout=1000 type=ssl_hello;
        #check interval=3000 rise=2 fall=5 timeout=1000 type=http;
        #check_http_send "GET / HTTP/1.0\r\n\r\n";
        #check_http_expect_alive http_2xx http_3xx;
    }
    server {
        listen 8888;
        proxy_pass cluster_www_ttlsa_com;
 so_keepalive on;
        tcp_nodelay on;
    }
}
```

nginx_tcp_proxy_module模块指令具体参见: <http://yaoweibin.github.io/nginx_tcp_proxy_module/README.html>


### nginx正向代理

我们平时用的最多的最常见的是反向代理。反向代理想必都会配置的，有不会的可以到本博客里面搜索下，有相关文档。 那么nginx的正向代理是如何配置的呢？
```
server {
 listen 8090;
 location / {
 resolver 218.85.157.99 218.85.152.99;
 resolver_timeout 30s;
 proxy_pass http://$host$request_uri;
 }
 access_log  /data/httplogs/proxy-$host-aceess.log;      
}
```

就这么简单哈。
测试：
<http://www.ttlsa.com:8090>
resolver指令
```
语法: resolver address ... [valid=time];
默认值: —
配置段: http, server, location
配置DNS服务器IP地址。可以指定多个，以轮询方式请求。
nginx会缓存解析的结果。默认情况下，缓存时间是名字解析响应中的TTL字段的值，可以通过valid参数更改。
resolver_timeout指令
语法: resolver_timeout time;
默认值: resolver_timeout 30s;
配置段: http, server, location
```
解析超时时间。
如需转载请注明出处：<http://www.ttlsa.com/html/3287.html>


### 搭建nginx反向代理用做内网域名转发


**情景**

由于公司内网有多台服务器的http服务要映射到公司外网静态IP，如果用路由的端口映射来做，就只能一台内网服务器的80端口映射到外网80端口，其他服务器的80端口只能映射到外网的非80端口。非80端口的映射在访问的时候要域名加上端口，比较麻烦。并且公司入口路由最多只能做20个端口映射。肯定以后不够用。
然后k兄就提议可以在内网搭建个nginx反向代理服务器，将nginx反向代理服务器的80映射到外网IP的80，这样指向到公司外网IP的域名的HTTP请求就会发送到nginx反向代理服务器，利用nginx反向代理将不同域名的请求转发给内网不同机器的端口，就起到了“根据域名自动转发到相应服务器的特定端口”的效果，而路由器的端口映射做到的只是“根据不同端口自动转发到相应服务器的特定端口”，真是喜大普奔啊。
涉及的知识：nginx编译安装，nginx反向代理基本配置，路由端口映射知识，还有网络域名等常识。
本次实验目标是做到：在浏览器中输入xxx123.tk能访问到内网机器192.168.10.38的3000端口，输入xxx456.tk能访问到内网机器192.168.10.40的80端口。
配置步骤

服务器ubuntu 12.04
\###更新仓库

```
apt-get update -y
apt-get install wget -y
```

\#下载nginx和相关软件包
pcre是为了编译rewrite模块，zlib是为了支持gzip功能。额，这里nginx版本有点旧，因为我还要做升级nginx的实验用。大家可以装新版本。

```
cd /usr/local/src
 wget <a href="ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.33.tar.gz">ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.33.tar.gz</a>
 wget <a href="http://zlib.net/zlib-1.2.8.tar.gz">http://zlib.net/zlib-1.2.8.tar.gz</a>
 wget <a href="http://nginx.org/download/nginx-1.4.2.tar.gz">http://nginx.org/download/nginx-1.4.2.tar.gz</a>
 tar xf pcre-8.33.tar.gz
 tar xf zlib-1.2.8.tar.gz
```

\#安装编译环境
```
apt-get install build-essential libtool -y
```

\#创建nginx用户
所谓的unprivileged user
```
useradd -s /bin/false -r -M -d /nonexistent www
```

\#开始编译安装

```
/configure --with-pcre=/usr/local/src/pcre-8.33 --with-zlib=/usr/local/src/zlib-1.2.8 --user=www --group=www \
 --with-http_stub_status_module --with-http_ssl_module --with-http_realip_module
 make
 make install
```

\#给文件夹授权

```
chown -R www:www /usr/local/nginx
```

#修改配置文件
vim nginx.conf
```
user www www;
worker_processes 1;
error_log logs/error.log;
pid logs/nginx.pid;
worker_rlimit_nofile 65535;
events {
    use epoll;
    worker_connections 65535;
}
http {
    include mime.types;
    default_type application/octet-stream;
    include /usr/local/nginx/conf/reverse-proxy.conf;
    sendfile on;
    keepalive_timeout 65;
    gzip on;
    client_max_body_size 50m; #缓冲区代理缓冲用户端请求的最大字节数,可以理解为保存到本地再传给用户
    client_body_buffer_size 256k;
    client_header_timeout 3m;
    client_body_timeout 3m;
    send_timeout 3m;
    proxy_connect_timeout 300s; #nginx跟后端服务器连接超时时间(代理连接超时)
    proxy_read_timeout 300s; #连接成功后，后端服务器响应时间(代理接收超时)
    proxy_send_timeout 300s;
    proxy_buffer_size 64k; #设置代理服务器（nginx）保存用户头信息的缓冲区大小
    proxy_buffers 4 32k; #proxy_buffers缓冲区，网页平均在32k以下的话，这样设置
    proxy_busy_buffers_size 64k; #高负荷下缓冲大小（proxy_buffers*2）
    proxy_temp_file_write_size 64k; #设定缓存文件夹大小，大于这个值，将从upstream服务器传递请求，而不缓冲到磁盘
    proxy_ignore_client_abort on; #不允许代理端主动关闭连接
    server {
        listen 80;
        server_name localhost;
        location / {
            root html;
            index index.html index.htm;
        }
        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
            root html;
        }
    }
}
```

编辑反向代理服务器配置文件：
vim /usr/local/nginx/conf/reverse-proxy.conf
```
server
{
    listen 80;
    server_name xxx123.tk;
    location / {
        proxy_redirect off;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_pass http://192.168.10.38:3000;
    }
    access_log logs/xxx123.tk_access.log;
}
 
server
{
    listen 80;
    server_name xxx456.tk;
    location / {
        proxy_redirect off;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_pass http://192.168.10.40:80;
    }
    access_log logs/xxx456.tk_access.log;
}
```

然后重新加载nginx配置文件，使之修改生效，再把xxx123.tk域名指向公司静态IP，这样就成功的做到了在浏览器中输入xxx123.tk的时候访问的内网服务器192.168.10.38的3000端口,输入xxx456.tk访问192.168.10.40的80端口的作用。
如果想对后端机器做负载均衡，像下面这配置就可以把对nagios.xxx123.tk的请求分发给内网的131和132这两台机器做负载均衡了。

```
upstream monitor_server {
    server 192.168.0.131:80;
        server 192.168.0.132:80;
}
 
server
{
    listen 80;
    server_name nagios.xxx123.tk;
    location / {
        proxy_redirect off;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; 
        proxy_pass http://monitor_server;
    }
    access_log logs/nagios.xxx123.tk_access.log;
}
```

额，关于负载均衡和缓存就不多说了，这里只是要起到一个简单的“域名转发”功能。
另外，由于http请求最后都是由反向代理服务器传递给后段的机器，所以后端的机器原来的访问日志记录的访问IP都是反向代理服务器的IP。
要想能记录真实IP，需要修改后端机器的日志格式，这里假设后端也是一台nginx：
在后端配置文件里面加入这一段即可：

```
log_format access '$HTTP_X_REAL_IP - $remote_user [$time_local] "$request" '
'$status $body_bytes_sent "$http_referer" '
'"$http_user_agent" $HTTP_X_Forwarded_For';
 
access_log logs/access.log access;
```

再看看原来日志的格式长什么样：
```
#log_format main '$remote_addr - $remote_user [$time_local] "$request" '
# '$status $body_bytes_sent "$http_referer" '
# '"$http_user_agent" "$http_x_forwarded_for"';
 
#access_log logs/access.log main;
```

看出区别了吧
 
遇到的问题

 
之前没配置下面这段，访问时候偶尔会出现504 gateway timeout，由于偶尔出现，所以不太好排查

```
proxy_connect_timeout 300s;
proxy_read_timeout 300s;
proxy_send_timeout 300s;
proxy_buffer_size 64k;
proxy_buffers 4 32k;
proxy_busy_buffers_size 64k;
proxy_temp_file_write_size 64k;
proxy_ignore_client_abort on;
```

报错日志：
```
...upstream timed out (110: Connection timed out) while reading response header from upstream, client: ...(后面的省略）
```

从日志看来是连接超时了，网上一通乱查之后估计可能是后端服务器响应超时了，本着大胆假设，小心求证的原则，既然假设了错误原因就要做实验重现错误：那就调整代理超时参数，反过来把代理超时阀值设小（比如1ms）看会不会次次出现504。后来发现把proxy_read_timeout 这个参数设置成1ms的时候，每次访问都出现504。于是把这个参数调大，加入上面那段配置，解决问题了。
作者邮箱：790455803@qq.com，有问题可以直接EMAIL作者，当然也可以加入我们ttlsa群单独私聊或者群里发提问。

### nginx+keepalived+proxy_cache 配置高可用nginx群集和高速缓存

环境：
CentOS release 5.8 192.168.10.108 cat
CentOS release 5.5 912.168.200.208
主调度器：192.168.10.108 192.169.10.251
备调度器：192.168.200.208 192.168.200.148
real ip :
192.169.10.251
192.168.200.148
vip : 192.168.10.104

一、在主备服务器上部署nginx
1、下载
wget <http://nginx.org/download/nginx-1.0.11.tar.gz>
wget <http://labs.frickle.com/files/ngx_cache_purge-1.4.tar.gz>

2、安装
```
yum -y install zlib-devel pcre-devel openssl-devel  # 安装依赖
tar –xvf ngx_cache_purge-1.4.tar.gz
tar –xvf nginx-1.0.11.tar.gz
cd nginx-1.0.11/
./configure --prefix=/usr/local/nginx --add-module=../ngx_cache_purge-1.4 --with-http_stub_status_module --with-http_ssl_module --with-http_flv_module --with-http_gzip_static_module
Make && make install
 
vi /usr/local/nginx/conf/nginx.conf
user nobody;
worker_processes 8;
#error_log logs/error.log error;
error_log /data/logs/error.log crit;
#error_log logs/error.log notice;
#error_log logs/error.log info;
#pid logs/nginx.pid;
events {
worker_connections 1024;
}
http {
include mime.types;
default_type application/octet-stream;
charset utf-8;
server_names_hash_bucket_size 128;
client_header_buffer_size 32k;
large_client_header_buffers 4 32k;
client_max_body_size 300m;
tcp_nopush on;
tcp_nodelay on;
client_body_buffer_size 512k;
proxy_connect_timeout 5;
proxy_read_timeout 60;
proxy_send_timeout 5;
proxy_buffer_size 16k;
proxy_buffers 4 64k;
proxy_busy_buffers_size 128k;
proxy_temp_file_write_size 128k;
#log_format main '$remote_addr - $remote_user [$time_local] "$request" '
# '$status $body_bytes_sent "$http_referer" '
# '"$http_user_agent" "$http_x_forwarded_for"';
#access_log logs/access.log main;
sendfile on;
#keepalive_timeout 65;
gzip on;
gzip_min_length 1k;
gzip_buffers 4 16k;
gzip_http_version 1.1;
gzip_comp_level 2;
gzip_types text/plain application/x-javascript text/css application/xml;
gzip_vary on;
proxy_temp_path /data/proxy_temp_dir;
proxy_cache_path /data/proxy_cache_dir levels=1:2 keys_zone=cache_one:50m inactive=1m max_size=2g;
upstream real_server_pool{
server 192.168.200.148:80 weight=1 max_fails=2 fail_timeout=30s;
server 192.168.10.251:80 weight=1 max_fails=2 fail_timeout=30s;
}
#tcp_nopush on;
#keepalive_timeout 0;
keepalive_timeout 65;
#gzip on;
server {
listen 80;
server_name localhost;
#charset koi8-r;
#access_log logs/host.access.log main;
location / {
root html;
index index.html index.htm;
proxy_next_upstream http_502 http_504 error timeout invalid_header;
proxy_cache cache_one;
proxy_cache_valid 200 304 12h;
proxy_cache_key $host$uri$is_args$args;
proxy_set_header Host $host;
proxy_set_header X-Forwarded-For $remote_addr;
proxy_pass http://real_server_pool;
expires 1d;
 
}
#error_page 404 /404.html;
# redirect server error pages to the static page /50x.html
#
error_page 500 502 503 504 /50x.html;
location = /50x.html {
root html;
}
# proxy the PHP scripts to Apache listening on 127.0.0.1:80
#
#location ~ \.php$ {
# proxy_pass http://127.0.0.1;
#}
location ~ .*\.(php|jsp|cgi)?$
{
proxy_set_header Host $host;
proxy_set_header X-Forwarded-For $remote_addr;
proxy_pass http://real_server_pool;
}
log_format access '$remote_addr - $remote_user [$time_local] "$request"'
'$status $body_bytes_sent "$http_referer" '
'"$http_user_agent" $http_x_forwarded_for';
access_log /data/logs/access.log access;
}
 
# pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
#
#location ~ \.php$ {
# root html;
# fastcgi_pass 127.0.0.1:9000;
# fastcgi_index index.php;
# fastcgi_param SCRIPT_FILENAME /scripts$fastcgi_script_name;
# include fastcgi_params;
#}
# deny access to .htaccess files, if Apache's document root
# concurs with nginx's one
#
#location ~ /\.ht {
# deny all;
#}
}
# another virtual host using mix of IP-, name-, and port-based configuration
#
#server {
# listen 8000;
# listen somename:8080;
# server_name somename alias another.alias;
# location / {
# root html;
# index index.html index.htm;
# }
#}
# HTTPS server
#
#server {
# listen 443;
# server_name localhost;
# ssl on;
# ssl_certificate cert.pem;
# ssl_certificate_key cert.key;
# ssl_session_timeout 5m;
# ssl_protocols SSLv2 SSLv3 TLSv1;
# ssl_ciphers HIGH:!aNULL:!MD5;
# ssl_prefer_server_ciphers on;
# location / {
# root html;
# index index.html index.htm;
# }
#}
```
备用调度器的nginx配置文件和主调度器的配置文件一样。
启动nginx
```
/usr/local/nginx/sbin/nginx
```
二、安装keepalived（在nginx的mater和backup都安装）
 
1、  下载
wget <http://www.keepalived.org/software/keepalived-1.1.19.tar.gz>
 
2、  安装
```
tar zxvf keepalived-1.1.19.tar.gz
cd keepalived-1.1.19
./configure --prefix=/usr/local/keepalived
make
make install
cp /usr/local/keepalived/sbin/keepalived /usr/sbin/
cp /usr/local/keepalived/etc/sysconfig/keepalived /etc/sysconfig/
cp /usr/local/keepalived/etc/rc.d/init.d/keepalived /etc/init.d/
mkdir /etc/keepalived
 
vi /etc/keepalived/keepalived.conf
vrrp_instance VI_INET1 {
state MASTER
interface eth0
virtual_router_id 53
priority 100
advert_int 1
authentication {
auth_type pass
auth_pass 1111
}
virtual_ipaddress {
192.168.10.104/24
}
}
virtual_server 192.168.10.104 80 {
delay_loop 6
lb_algo rr
lb_kind NAT
nat_mask 255.255.255.0
persistence_timeout 50
protocol TCP
real_server 192.168.10.251 80 {
weight 3
TCP_CHECK {
connect_timeout 10
nb_get_retry 3
delay_before_retry 3
connect_port 80
}
}
real_server 192.168.200.148 80 {
weight 3
TCP_CHECK {
connect_timeout 10
nb_get_retry 3
delay_before_retry 3
connect_port 80
}
}
}
```
4、配置备用调度器的keepalived,只需要将state MASTER 改为state BACKUP,降低priority 100 的值:
```
state MASTER ---> state BACKUP
priority 100 --->  priority 99 （此值必须低于主的）
```
主备启动
```
/etc/init.d/keepalived start
```
三、测试
建立虚拟主机（自己测试啊    O(∩_∩)O~）


### Nginx 战斗准备 —— 优化指南

大多数的Nginx安装指南告诉你如下基础知识——通过apt-get安装，修改这里或那里的几行配置，好了，你已经有了一个Web服务器了！而且，在大多数情况下，一个常规安装的nginx对你的网站来说已经能很好地工作了。然而，如果你真的想挤压出nginx的性能，你必须更深入一些。在本指南中，我将解释Nginx的那些设置可以微调，以优化处理大量客户端时的性能。需要注意一点，这不是一个全面的微调指南。这是一个简单的预览——那些可以通过微调来提高性能设置的概述。你的情况可能不同。

**基本的 (优化过的)配置**

我们将修改的唯一文件是nginx.conf，其中包含Nginx不同模块的所有设置。你应该能够在服务器的/etc/nginx目录中找到nginx.conf。首先，我们将谈论一些全局设置，然后按文件中的模块挨个来，谈一下哪些设置能够让你在大量客户端访问时拥有良好的性能，为什么它们会提高性能。本文的结尾有一个完整的配置文件。

**高层的配置**

nginx.conf文件中，Nginx中有少数的几个高级配置在模块部分之上。

```
user www-data;
pid /var/run/nginx.pid;
worker_processes auto;
worker_rlimit_nofile 100000;
```

user和pid应该按默认设置 - 我们不会更改这些内容，因为更改与否没有什么不同。

worker_processes 定义了nginx对外提供web服务时的worder进程数。最优值取决于许多因素，包括（但不限于）CPU核的数量、存储数据的硬盘数量及负载模式。不能确定的时候，将其设置为可用的CPU内核数将是一个好的开始（设置为“auto”将尝试自动检测它）。

worker_rlimit_nofile 更改worker进程的最大打开文件数限制。如果没设置的话，这个值为操作系统的限制。设置后你的操作系统和Nginx可以处理比“ulimit -a”更多的文件，所以把这个值设高，这样nginx就不会有“too many open files”问题了。

**Events模块**

events模块中包含nginx中所有处理连接的设置。

```
events {
worker_connections 2048;
multi_accept on;
use epoll;
}
```

worker_connections设置可由一个worker进程同时打开的最大连接数。如果设置了上面提到的worker_rlimit_nofile，我们可以将这个值设得很高。
记住，最大客户数也由系统的可用socket连接数限制（~ 64K），所以设置不切实际的高没什么好处。

multi_accept 告诉nginx收到一个新连接通知后接受尽可能多的连接。

use 设置用于复用客户端线程的轮询方法。如果你使用Linux 2.6+，你应该使用epoll。如果你使用*BSD，你应该使用kqueue。想知道更多有关事件轮询？看下维基百科吧（注意，想了解一切的话可能需要neckbeard和操作系统的课程基础）
（值得注意的是如果你不知道Nginx该使用哪种轮询方法的话，它会选择一个最适合你操作系统的）


**HTTP 模块**

HTTP模块控制着nginx http处理的所有核心特性。因为这里只有很少的配置，所以我们只节选配置的一小部分。所有这些设置都应该在http模块中，甚至你不会特别的注意到这段设置。
```
http {
server_tokens off;
sendfile on;
 tcp_nopush on;
tcp_nodelay on;
...
}
```


server_tokens 并不会让nginx执行的速度更快，但它可以关闭在错误页面中的nginx版本数字，这样对于安全性是有好处的。sendfile可以让sendfile()发挥作用。sendfile()可以在磁盘和TCP socket之间互相拷贝数据(或任意两个文件描述符)。Pre-sendfile是传送数据之前在用户空间申请数据缓冲区。之后用read()将数据从文件拷贝到这个缓冲区，write()将缓冲区数据写入网络。sendfile()是立即将数据从磁盘读到OS缓存。因为这种拷贝是在内核完成的，sendfile()要比组合read()和write()以及打开关闭丢弃缓冲更加有效(更多有关于sendfile)

tcp_nopush 告诉nginx在一个数据包里发送所有头文件，而不一个接一个的发送

tcp_nodelay 告诉nginx不要缓存数据，而是一段一段的发送--当需要及时发送数据时，就应该给应用设置这个属性，这样发送一小块数据信息时就不能立即得到返回值

```
access_log off;
error_log /var/log/nginx/error.log crit;
```

access_log设置nginx是否将存储访问日志。关闭这个选项可以让读取磁盘IO操作更快(aka,YOLO)error_log 告诉nginx只能记录严重的错误
```
keepalive_timeout 10;
client_header_timeout 10;
client_body_timeout 10;
reset_timedout_connection on;
send_timeout 10;
```

keepalive_timeout 给客户端分配keep-alive链接超时时间。服务器将在这个超时时间过后关闭链接。我们将它设置低些可以让ngnix持续工作的时间更长。client_header_timeout 和client_body_timeout 设置请求头和请求体(各自)的超时时间。我们也可以把这个设置低些。

reset_timeout_connection告诉nginx关闭不响应的客户端连接。这将会释放那个客户端所占有的内存空间。

send_timeout 指定客户端的响应超时时间。这个设置不会用于整个转发器，而是在两次客户端读取操作之间。如果在这段时间内，客户端没有读取任何数据，nginx就会关闭连接。
```
limit_conn_zone $binary_remote_addr zone=addr:5m;
limit_conn addr 100;
```

limit_conn_zone设置用于保存各种key（比如当前连接数）的共享内存的参数。5m就是5兆字节，这个值应该被设置的足够大以存储（32K*5）32byte状态或者（16K*5）64byte状态。limit_conn为给定的key设置最大连接数。这里key是addr，我们设置的值是100，也就是说我们允许每一个IP地址最多同时打开有100个连接。

```
include /etc/nginx/mime.types;
default_type text/html;
charset UTF-8;
```

include只是一个在当前文件中包含另一个文件内容的指令。这里我们使用它来加载稍后会用到的一系列的MIME类型。default_type设置文件使用的默认的MIME-type。
charset设置我们的头文件中的默认的字符集
以下两点对于性能的提升在伟大的WebMasters StackExchange中有解释。

```
gzip on;
gzip_disable "msie6";
# gzip_static on;
gzip_proxied any;
gzip_min_length 1000;
gzip_comp_level 4;
gzip_types text/plain text/css application/json application/x-javascript text/xml application/xml application/xml+rss text/javascript;
```

gzip是告诉nginx采用gzip压缩的形式发送数据。这将会减少我们发送的数据量。

gzip_disable为指定的客户端禁用gzip功能。我们设置成IE6或者更低版本以使我们的方案能够广泛兼容。

gzip_static告诉nginx在压缩资源之前，先查找是否有预先gzip处理过的资源。这要求你预先压缩你的文件（在这个例子中被注释掉了），从而允许你使用最高压缩比，这样nginx就不用再压缩这些文件了（想要更详尽的gzip_static的信息，请点击这里）。

gzip_proxied允许或者禁止压缩基于请求和响应的响应流。我们设置为any，意味着将会压缩所有的请求。

gzip_min_length设置对数据启用压缩的最少字节数。如果一个请求小于1000字节，我们最好不要压缩它，因为压缩这些小的数据会降低处理此请求的所有进程的速度。

gzip_comp_level设置数据的压缩等级。这个等级可以是1-9之间的任意数值，9是最慢但是压缩比最大的。我们设置为4，这是一个比较折中的设置。

gzip_type设置需要压缩的数据格式。上面例子中已经有一些了，你也可以再添加更多的格式。

```
# cache informations about file descriptors, frequently accessed files
# can boost performance, but you need to test those values
open_file_cache max=100000 inactive=20s;
open_file_cache_valid 30s;
open_file_cache_min_uses 2;
open_file_cache_errors on;
##
# Virtual Host Configs
# aka our settings for specific servers
##
include /etc/nginx/conf.d/*.conf;
include /etc/nginx/sites-enabled/*;
```

open_file_cache打开缓存的同时也指定了缓存最大数目，以及缓存的时间。我们可以设置一个相对高的最大时间，这样我们可以在它们不活动超过20秒后清除掉。
open_file_cache_valid 在open_file_cache中指定检测正确信息的间隔时间。
open_file_cache_min_uses 定义了open_file_cache中指令参数不活动时间期间里最小的文件数。
open_file_cache_errors指定了当搜索一个文件时是否缓存错误信息，也包括再次给配置中添加文件。我们也包括了服务器模块，这些是在不同文件中定义的。如果你的服务器模块不在这些位置，你就得修改这一行来指定正确的位置。

**一个完整的配置**

```
user www-data;
pid /var/run/nginx.pid;
worker_processes auto;
worker_rlimit_nofile 100000;
events {
 worker_connections 2048;
 multi_accept on;
 use epoll;
}
http {
  server_tokens off;
 sendfile on;
tcp_nopush on;
 tcp_nodelay on;
 access_log off;
 error_log /var/log/nginx/error.log crit;
 keepalive_timeout 10;
 client_header_timeout 10;
 client_body_timeout 10;
reset_timedout_connection on;
 send_timeout 10;
 limit_conn_zone $binary_remote_addr zone=addr:5m;
 limit_conn addr 100;
 include /etc/nginx/mime.types;
 default_type text/html;
charset UTF-8;
gzip on;
 gzip_disable "msie6";
gzip_proxied any;
gzip_min_length 1000;
gzip_comp_level 6;
 gzip_types text/plain text/css application/json application/x-javascript text/xml application/xml application/xml+rss text/javascript;
 open_file_cache max=100000 inactive=20s;
open_file_cache_valid 30s;
open_file_cache_min_uses 2;
open_file_cache_errors on;
 include /etc/nginx/conf.d/*.conf;
 include /etc/nginx/sites-enabled/*;
}
```

编辑完配置后，确认重启nginx使设置生效。
后记

就这样！你的Web服务器现在已经就绪，之前困扰你的众多访问者的问题来吧。这并不是加速网站的唯一途径，很快我会写更多介绍其他加速网站方法的文章的。

### 确保nginx安全的10个技巧

Nginx是当今最流行的Web服务器之一。它为世界上7%的web流量提供服务而且正在以惊人的速度增长。它是个让人惊奇的服务器，我愿意部署它。
下面是一个常见安全陷阱和解决方案的列表，它可以辅助来确保你的Nginx部署是安全的。

**1. 在配置文件中小心使用"if"**

它是重写模块的一部分，不应该在任何地方使用。
“if”声明是重写模块评估指令强制性的部分。换个说法，Nginx的配置一般来说是声明式的。在有些情况下，由于用户的需求，他们试图在一些非重写指令内使用“if”，这导致我们现在遇到的情况。大多数情况下都能正常工作，但…看上面提到的。
看起来唯一正确的解决方案是在非重写的指令内完全禁用“if”。这将更改现有的许多配置，所以还没有完成。IfIsEvil：http://wiki.nginx.org/IfIsEvil

**2. 将每个~ .php$请求转递给PHP**

我们上周发布了这个流行指令的潜在安全漏洞介绍。即使文件名为hello.php.jpeg它也会匹配~ .php$这个正则而执行文件。
现在有两个解决上述问题的好方法。我觉得确保你不轻易执行任意代码的混合方法很有必要。
2.1 如果没找到文件时使用try_files和only(在所有的动态执行情况下都应该注意) 将它转递给运行PHP的FCGI进程。
2.2 确认php.ini文件中cgi.fix_pathinfo设置为0 (cgi.fix_pathinfo=0) 。这样确保PHP检查文件全名(当它在文件结尾没有发现.php它将忽略)
2.3 修复正则表达式匹配不正确文件的问题。现在正则表达式认为任何文件都包含".php"。在站点后加“if”确保只有正确的文件才能运行。将/location ~ .php$和location ~ ..*/.*.php$都设置为return 403;

**3. 禁用autoindex模块**

这个可能在你使用的Nginx版本中已经更改了，如果没有的话只需在配置文件的location块中增加autoindex off;声明即可。

**4. 禁用服务器上的ssi (服务器端引用)**

这个可以通过在location块中添加ssi off; 。

**5. 关闭服务器标记**

如果开启的话（默认情况下）所有的错误页面都会显示服务器的版本和信息。将server_tokens off;声明添加到Nginx配置文件来解决这个问题。

**6. 在配置文件中设置自定义缓存以限制缓冲区溢出攻击的可能性**

```
client_body_buffer_size 1K;
client_header_buffer_size 1k;
client_max_body_size 1k;
large_client_header_buffers 2 1k;
```


**7. 将timeout设低来防止DOS攻击**

所有这些声明都可以放到主配置文件中。
```
client_body_timeout 10;
client_header_timeout 10;
keepalive_timeout 5 5;
send_timeout 10;
```

**8. 限制用户连接数来预防DOS攻击**
```
limit_zone slimits $binary_remote_addr 5m;
limit_conn slimits 5;
```

**9. 试着避免使用HTTP认证**

HTTP认证默认使用crypt，它的哈希并不安全。如果你要用的话就用MD5（这也不是个好选择但负载方面比crypt好） 。

**10. 保持与最新的Nginx安全更新**
转自：<http://www.levigross.com/post/4488812448/10-tips-for-securing-nginx>
个人觉得在防止DDOS攻击这方面，上面提到的第七第八没太大用处，特别是第八点，很扰乱用户体验度的。







## 解惑

### 配置中变量解析问题
```
return 413 '$request_filename';
return 413 "$request_filename";
return 413 "${request_filename}";
return 413 '${request_filename}';
return 413 $request_filename;
return 413 "'$request_filename'";
return 413 '"$request_filename"';
//以上所有，变量$request_filename都能被解析，当然单双引号同时包裹的，结果会有外层引号
return 413 ${request_filename}; //配置报错
```
**备注：测试时回复状态码不要用3××，这是告诉浏览器资源跳转，可以采用4××,5××;**

### location配置
```
location / {
            return 555;
            index  index.html index.htm index.php l.php;
            if (!-e $request_filename) {
              rewrite  ^(.*)$  /index.php?s=/$1  last;
              break;
            }
            autoindex  off;
        }
location ~ /data {
          return 556 '响应体内容';//注意：如果uri不是访问http://localhost/overlord.htm?s=111，即没有文件名后缀，则不能有响应体，不然会无法访问，服务器也不会响应状态码
        }
```
说明：
- 实践得知：正则模式~和~\*都不区分大小写
- 实践得知：省略正则模式，默认为模式~



## 实际操作

### url重写，去掉index.php
```
server { 
    listen       80;
    default_type text/plain;
    root /var/www/html;
    index index.php index.htm index.html;
	  #隐藏index.php，location / 代表匹配域名后的所有，这个容易在本地搞错
    location / { 				
          if (!-e $request_filename) {
                   #一级目录
                   rewrite ^/(.*)$ /index.php/$1 last;
                   #二级目录
                   #rewrite ^/MYAPP/(.*)$ /MYAPP/index.php/$1 last;
             }  
    }
    #pathinfo设置
    location ~ \.php($|/) {
        fastcgi_pass   127.0.0.1:9000;
        fastcgi_index  index.php;
        fastcgi_split_path_info ^(.+\.php)(.*)$;
        fastcgi_param   PATH_INFO $fastcgi_path_info;
        fastcgi_param  SCRIPT_FILENAME   $document_root$fastcgi_script_name;
        include        fastcgi_params;
    }
}
```

## 问题

### 在windows下可以正常使用的代码，在linux下无法登陆，即session无法被赋值或者无法跨页面使用

解决：是因为采用php-fpm后，session保存位置并不是在php.ini中的session.save_path设置，而是在/etc/php-fpm.d/www.conf文件中的`php_value[session.save_path]    = /var/lib/php/session` 设置的。需要查看这个配置设置的目录用户和用户组与php-fpm配置的user和group配置是否一致。
