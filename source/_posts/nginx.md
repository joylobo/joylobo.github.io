---
title: Nginx学习笔记
date: 2021-03-04 10:18:38
tags:
    - 中间件
    - 反向代理
    - 负载均衡
---

{% raw %}
    <style type="text/css">
        table > thead > tr > th:nth-child(1) {
            width: 250px;
        }
    </style>
{% endraw %}

# 简介
Nginx服务器以其功能丰富著称于世。它既可以作为HTTP服务器，也可以作为反向代理服务器或者邮件服务器；能够快速响应静态页面(HTML)的请求;支持FastCGI、SSL、Virtual Host、URL Rewrite、HTTP Basic Auth、Gzip等大量实用功能；并支持更多的第三方功能模块的扩展。

## 功能特性
### 基本HTTP服务
在Nginx提供的基本HTTP服务组件中，主要包含以下功能特性：
* 处理静态文件(如HTML静态网页及请求);处理索引文件以及支持自动索引。
* 打开并自行管理文件描述符缓存。
* 提供反向代理服务，并且可以使用缓存加速反向代理，同时完成简单负载均衡及容错。
* 提供远程FastCGI服务的缓存机制，加速访问，同时完成简单的负载均衡及容错。
* 使用Nginx的模块化特性提供过滤器功能。Nginx基本过滤器包括gzip压缩、ranges支持、chunked响应，XSLT、SSI以及图像缩放等。其中，针对包含多个SSI的页面，经由FastCGI或反向代理，SSI过滤器可以并行处理。
* 支持HTTP下的安全套接层安全协议SSL。

### 高级HTTP服务
在Nginx提供的高级HTTP服务中，主要包含以下功能特性：
* 支持基于名字和IP的虚拟主机设置。
* 支持HTTP/1.0中KEEP-ALIVE模式和管线(PipeLined)模型连接。
* 支持重新加载配置以及在线升级时，无需中断正在处理的请求。
* 自定义访问日志格式、带缓存的日志写操作以及快速日志轮转。
* 提供3xx~5xx错误代码重定向功能。
* 支持重写(Rewrite)模块扩展。
* 支持HTTP DAV模块，从而为HTTP WebDAV提供PUT、DELETE、MKCOL、COPY已经MOVE方法。
* 支持FLV流和MP4流传输。
* 支持网络监控，包括基于客户端IP地址和HTTP基本认证机制的访问控制、速度限制、来自同一地址的同时连接数或请求数限制等。
* 支持嵌入Perl语言。

### 邮件代理服务
Nginx提供邮件代理服务也是其基本开发需求之一，主要包含以下功能特性：
* 支持使用外部HTTP认证服务器重定向用户到IMAP/POP3后端，并支持IMAP认证方式(LOGIN、AUTH LOGIN/CRAM-MD5)和POP3认证方式(USER/PASS、APOP、AUTH LOGIN/PLAIN/CRAM-MD5)。
* 支持使用外部HTTP认证服务器认证用户重定向连接到内部SMTP后端，并支持SMTP认证方式(AUTH LOGIN/PLAIN/CRAM-MD5)。
* 支持邮件代理服务下的安全套接层协议SSL。
* 支持纯文本通信协议的扩展协议STARTTLS。

## 常用功能介绍
### HTTPS代理和反向代理
代理服务和反向代理服务是Nginx服务器作为Web服务器的主要功能之一，尤其是反向代理服务，是应用十分广泛的功能。在提供反向代理服务方面，Nginx服务器转发前端请求性能稳定，并且后端转发与业务配置相互分离，配置相当灵活。

### 负载均衡
负载均衡，一般包含两方面含义。一方面是，将单一的负载分担到多个网络节点上并行处理，每个节点处理结束后将结果汇总给用户，这样可以大幅提高网络系统的处理能力;第二个方面的含义是，将大量的前端并发访问或数据流量分担到多个后端网络节点处理，这样可以有效减少前端用户等待响应的时间。Web服务器，FTP服务器、企业关键应用服务器等网络应用方面谈到的负载均衡问题，基本隶属于后一方面的含义。

### Web缓存
Nginx服务器的Web缓存服务主要由Proxy_Cache相关指令集和FastCGI相关指令集构成。其中，Proxy_Cache主要用于在Nginx服务器提供反向代理服务时，对后端源服务器的返回内容进行URL缓存;FastCGI_Cache主要用于对FastCGI的动态程序进行缓存。另外还有一款常用的第三方模块ngx_cache_purge也是Nginx服务器Web缓存功能中常用到的。它主要用于清除Nginx服务上指定的URL缓存。

# 安装和配置
### 编译

| 选项 | 说明 |
| --- | --- | 
| --prefix=&lt;path&gt; | 指定Nginx软件的安装路径，默认为/usr/local/nginx/目录 |
| --sbin-path=&lt;path&gt; | 指定Nginx可执行文件的安装路径，默认为&lt;prefix&gt;/sbin/nginx/目录。 |
| --config-path=&lt;path&gt; | 在未给定-c选项下，指定默认的nginx.conf路径，默认为&lt;prefix&gt;/conf/目录。 |
| --pid-path=&lt;path&gt; | 在nginx.conf中未指定pid指令的情况下，指定默认的nginx.pid路径，默认为&lt;prefix&gt;/logs/nginx.pid。nginx.pid保存了当前运行的Nginx服务的进程号 |
| --lock-path=&lt;path&gt; | 指定nginx.lock文件的路径，默认为/var/lock/目录。nginx.lock是Nginx服务器的锁文件。 |
| --error-log-path=&lt;path&gt; | 在nginx.conf中未指定error_log的情况下，指定默认的错误日志的路径，默认为&lt;prefix&gt;/logs/error.log |
| --http-log-path=&lt;path&gt; | 在nginx.conf中未指定access_log指令的情况下，指定默认的访问日志路径，默认为&lt;prefix&gt;/logs/access.log |
| --user=&lt;user&gt; | 在nginx.conf中未指定user指令的情况下，指定默认的Nginx服务器进程的属主用户，即Nginx进程运行的用户。可以理解为指定哪个用户启动Nginx，默认为nobody，表示不限制 |
| --group=&lt;group&gt; | 在nginx.conf中未指定user指令的情况下，指定默认的Nginx服务器进程的属主用户组，即Nginx进程运行的用户组。可以理解为指定哪个用户组的用户启动Nginx，默认为nobody，表示不限制 |
| --builddir=&lt;dir&gt; | 指定编译时的目录 |
| --with-debug | 声明启用Nginx的调用日志 |
| --add-module=&lt;path&gt; | 指定第三方模块的路径，用以编译到Nginx服务器中。 |
| --with-poll_module | 声明启用poll模块，poll模块是信号处理的一种方法，和下面提到的select模式类似，都是采用轮训方法处理信号 |
| --without-poll_module | 声明禁止poll模块 |
| --with-select_module | 声明启用select信号处理模式，若configure未找到指定其他的信号处理模式(如SUN系统中的kqueue，Linux内核的epoll、实时信号rtsig以及和select类似的/dev/poll等)，则默认使用select模式 |
| --without-select_module | 声明禁止select信号处理模式 |
| --with-http_ssl_module | 声明启用HTTP的ssl模块，这样Nginx服务器就可以支持HTTPS请求了，这个模块的正常运行需要安装openssl库 |
| --with-http_realip_module | 声明启用HTTP的realip模块，默认不启用 |
| --with-http_addition_module | 声明启用HTTP的addition模块，默认不启用 |
| --with-http_sub_module | 声明启用HTTP的sub模块，默认不启用 |
| --with-http_dav_module | 声明启用HTTP的dav模块，默认不启用 |
| --with-http_flv_module | 声明启用HTTPS的flv模块，默认不启用。flv模块使得Nginx服务器支持flv流媒体的传输 |
| --with-http_stub_status_module | 声明启用Server Status页，默认不启用 |
| --with-http_perl_module | 声明启用HTTP的perl模块，默认不启用。perl模块使得Nginx服务器支持perl脚本的运行 |
| --with-perl_modules_path=&lt;path&gt; | 指定perl模块的路径 |
| --with-perl=&lt;path&gt; | 指定perl执行文件的路径 |
| --without-http_charset_module | 声明禁止HTTP的charset模块，默认启用 |
| --without-http_gzip_module | 声明禁用HTTP的gzip模块，默认启用。使用gzip模块需要安装zlib库 |
| --without-http_ssi_module | 声明禁用HTTP的ssi模块，默认启用 |
| --without-http_userid_module | 声明禁用HTTP的userid模块，默认启用 |
| --without-http_access_module | 声明禁用HTTP的access模块， 默认启用 |
| --without-http_auth_basic_module | 声明禁用HTTP的auth basic模块，默认启用 |
| --without-http_autoindex_module | 声明禁用HTTP的autoindex模块，默认启用 |
| --without-http_geo_module | 声明禁用HTTP的geo模块，默认启用 |
| --without-http_map_module | 声明禁用HTTP的map模块，默认启用 |
| --without-http_referer_module | 声明禁用HTTP的referer模块，默认启用 |
| --without-http_rewrite_module | 声明禁用HTTP的rewrite模块，默认启用。使用rewrite模块需要安装pcre库 |
| --without-http_proxy_module | 声明禁用HTTP的proxy模块，默认启用 |
| --without-http_fastcgi_module | 声明禁用HTTP的fastcgi模块，默认启用 |
| --without-http_memcached_module | 声明禁用HTTP的memcached模块，默认启用 |

### 启动和停止

### 配置
