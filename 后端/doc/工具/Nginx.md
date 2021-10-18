# Nginx

### 什么是nginx

nginx 是一个web 服务器和反向代理服务器 用于HTTP HTTPS SMTP POP3 和IMAP 协议

### nginx 的特性

- 反向代理/ L7 负载均衡
- 嵌入式 Perl 解释器
- 动态二进制升级
- 可用于重新编写URL ，具有非常好的PCRE 支持

### 请解释Nginx 如果处理HTTP 请求

Nginx 使用反应器模式，主事件循环等待操作系统发出准备事件的信号，这样数据就可以从套接字读取。 在该实例中读取到缓冲区并进行处理，单个线程可以提供数万个并发连接

### 什么是正向代理和反向代理

- 正向代理就是一个人发送一个请求直接就到达了目标的服务器
- 反方代理就是请求统一被Nginx接收，nginx反向代理服务器接收到之后，按照一定的规则分发给了后端的业务处理服务器进行处理了

### 使用反向代理服务器的优点是什么

反向代理服务器可以隐藏资源服务器的存在和特征，它充当互联网云和web 服务器之间的中间层。 这对安全方面来说是很好的。特别是使用web 托管服务时

### nginx 服务器的用途

nginx 服务器的最佳用法是在网络商部署动态HTTP 内容， 使用SCGI WSGI 应用程序服务器，用于脚本的FastCGI 处理程序。 还可以作为负载均衡器

### nginx  服务器上的master 和worker 进程分别是什么

- master 进程  ：  读取及评估配置和维持
- worker进程：  处理请求

### 是否可能讲nginx 的错误替换为502 503 错误

502 === 错误网关 

503 === 服务器超载

有可能， 需要确保 fastcgi_intercept_errors 被设置为 ON，并使用错误页面指令

Location / {fastcgi_pass 127.0.01:9001;fastcgi_intercept_errorson;error_page 502 =503/error_page.html;#…}

### 请解释  ngx_http_upstream_module  的作用是什么

ngx_http_upstream_module 用于定义可通过 fastcgi 传递、proxy 传递、uwsgi传递、memcached 传递和 scgi 传递指令来引用的服务器组

### 请解释什么是 C10K  问题

   C10K 问题是指无法同时处理大量客户端(10,000)的网络套接字

### 解释 x Nginx  是否支持将请求压缩到上游

您可以使用 Nginx 模块 gunzip 将请求压缩到上游。gunzip 模块是一个过滤器，它可以对不支持“gzip”编码方法的客户机或服务器使用“内容编码:gzip”来解压缩响应

### 为什么Nginx性能这么高

因为他的事件处理机制：异步非阻塞事件处理机制：运用了epoll模型，提供了一个队列，排队解决

### Nginx负载均衡的算法怎么实现的?策略有哪些

为了避免服务器崩溃，大家会通过负载均衡的方式来分担服务器压力。将对台服务器组成一个集群，当用户访问时，先访问到一个转发服务器，再由转发服务器将访问分发到压力更小的服务器

1. 轮询(默认)  每个请求按时间顺序逐一分配到不同的后端服务器，如果后端某个服务器宕机，能自动剔除故障系统

   ```shell
   upstream backserver {
   server 192.168.0.12;
   server 192.168.0.13;
   }
   ```

2. 权重 weight 

   weight的值越大分配  到的访问概率越高，主要用于后端每台服务器性能不均衡的情况下。其次是为在主从的情况下设置不同的权值，达到合理有效的地利用主机资源 .权重越高，在被访问的概率越大

   ```shell
   upstream backserver {
   server 192.168.0.12 weight=2;
   server 192.168.0.13 weight=8;
   }	
   ```

   

3. ip_hash( IP绑定)   每个请求按访问IP的哈希结果分配，使来自同一个IP的访客固定访问一台后端服务器，并且可以有效解决动态网页存在的session共享问题

   ```shell
   upstream backserver {
   ip_hash;
   server 192.168.0.12:88;
   server 192.168.0.13:80;
   }
   ```

4. fair(第三方插件)

   必须安装upstream_fair模块。对比 weight、ip_hash更加智能的负载均衡算法，fair算法可以根据页面大小和加载时间长短智能地进行负载均衡，响应时间短的优先分配

   ```shell
   upstream backserver {
   server server1;
   server server2;
   fair;
   }
   ```

5. url_hash(第三方插件)

   必须安装Nginx的hash软件包按访问url的hash结果来分配请求，使每个url定向到同一个后端服务器，可以进一步提高后端缓存服务器的效率

   ```shell
   upstream backserver {
   server squid1:3128;
   server squid2:3128;
   hash $request_uri;
   hash_method crc32;
   }
   ```

### Nginx配置文件nginx.conf有哪些属性模块

```shell
worker_processes 1；              # worker进程的数量
events {                     # 事件区块开始
 worker_connections 1024；         # 每个worker进程支持的最大连接数
}                      # 事件区块结束
http {                    # HTTP区块开始
 include    mime.types；           # Nginx支持的媒体类型库文件
 default_type application/octet-stream；      # 默认的媒体类型
 sendfile    on；            # 开启高效传输模式
 keepalive_timeout 65；          # 连接超时
 server {                  # 第一个Server区块开始，表示一个独立的
虚拟主机站点
   listen    80；             # 提供服务的端口，默认80
   server_name localhost；        # 提供服务的域名主机名
   location / {              # 第一个location区块开始
     root  html；          # 站点的根目录，相当于Nginx的安装目录
     index index.html index.htm；      # 默认的首页文件，多个用空格分开
   }                 # 第一个location区块结果
   error_page  500502503504 /50x.html；     # 出现对应的http状态码时，使
用50x.html回应客户
   location = /50x.html {           # location区块开始，访问
50x.html
     root  html；               # 指定对应的站点目录为html
   }
 }
```

### Nginx怎么处理请求的

```shell
server {              # 第一个Server区块开始，表示一个独立的虚拟主机
站点
   listen    80；           # 提供服务的端口，默认80
   server_name localhost；      # 提供服务的域名主机名
   location / {            # 第一个location区块开始
     root  html；        # 站点的根目录，相当于Nginx的安装目录
     index index.html index.htm；    # 默认的首页文件，多个用空格分开
   }             # 第一个location区块结果
 } 
```

### Nginx虚拟主机怎么配置

1. 基于域名的虚拟主机，通过域名来区分虚拟主机——应用：外部网站

   需要建立/data/www /data/bbs目录，windows本地hosts添加虚拟机ip地址对应的域名解析；对应域名
   网站目录下新增index.html文件

   ```shell
   #当客户端访问www.lijie.com,监听端口号为80,直接跳转到data/www目录下文件
    server {
      listen    80;
      server_name www.lijie.com;
      location / {
        root  data/www;
        index index.html index.htm;
      }
    }
    #当客户端访问www.lijie.com,监听端口号为80,直接跳转到data/bbs目录下文件
    server {
      listen    80;
      server_name bbs.lijie.com;
      location / {
        root  data/bbs;
        index index.html index.htm;
      }
    }
   ```

   

2. 基于端口的虚拟主机，通过端口来区分虚拟主机——应用：公司内部网站，外部网站的管理后台

   ```shell
   #当客户端访问www.lijie.com,监听端口号为8080,直接跳转到data/www目录下文件
    server {
      listen    8080;
      server_name 8080.lijie.com;
      location / {
        root  data/www;
        index index.html index.htm;
      }
    }
    #当客户端访问www.lijie.com,监听端口号为80直接跳转到真实ip服务器地址 127.0.0.1:8080
    server {
      listen    80;
      server_name www.lijie.com;
      location / {
      proxy_pass http://127.0.0.1:8080;
          index index.html index.htm;
      }
    }
   ```

   

3. 基于ip的虚拟主机



### location的作用是什么？

location指令的作用是根据用户请求的URI来执行不同的应用，也就是根据用户请求的网站URL进行匹配，匹配成功即进行相关的操作

```shell
#优先级1,精确匹配，根路径
 location =/ {
   return 400;
 }
 #优先级2,以某个字符串开头,以av开头的，优先匹配这里，区分大小写
 location ^~ /av {
   root /data/av/;
 }
 #优先级3，区分大小写的正则匹配，匹配/media*****路径
 location ~ /media {
    alias /data/static/;
 }
 #优先级4 ，不区分大小写的正则匹配，所有的****.jpg|gif|png 都走这里
 location ~* .*\.(jpg|gif|png|js|css)$ {
   root /data/av/;
 }
 #优先7，通用匹配
 location / {
   return 403;
 }
```

### 限流怎么做的

Nginx限流就是限制用户请求速度，防止服务器受不了 限流有3种

1. 正常限制访问频率（正常流量）

   限制一个用户发送的请求，我Nginx多久接收一个请求。
   Nginx中使用ngx_http_limit_req_module模块来限制的访问频率，限制的原理实质是基于漏桶算法原理来实现的。在nginx.conf配置文件中可以使用limit_req_zone命令及limit_req命令限制单个IP的请求处理频率

2. 突发限制访问频率（突发流量）

   限制一个用户发送的请求，我Nginx多久接收一个。
   上面的配置一定程度可以限制访问频率，但是也存在着一个问题：如果突发流量超出请求被拒绝处理，无法处理活动时候的突发流量，这时候应该如何进一步处理呢？
   Nginx提供burst参数结合nodelay参数可以解决流量突发的问题，可以设置能处理的超过设置的请求数外能额外处理的请求数。我们可以将之前的例子添加burst参数以及nodelay参数

3. 限制并发连接数

   Nginx中的ngx_http_limit_conn_module模块提供了限制并发连接数的功能，可以使用limit_conn_zone指令以及limit_conn执行进行配置

