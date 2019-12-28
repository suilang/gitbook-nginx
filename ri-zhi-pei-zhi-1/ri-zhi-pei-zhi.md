---
description: >-
  nginx日志共三个参数：access_log: 定义日志的路径及格式。log_format: 定义日志的模板。open_log_file_cache:
  定义日志文件缓存。
---

# 日志参数配置

## **一、**access配置

### 1.1 log\_format 定义日志格式

语法格式:

```text
 log_format name [escape=default|json] string …; 
 
 # 作用域 : http
 # 示例
 # log_format compression '$remote_addr - $remote_user [$time_local] '
                       '"$request" $status $bytes_sent '
                       '"$http_referer" "$http_user_agent" "$gzip_ratio"';
```

### 1.2 access\_log日志配置

access\_log用来定义日志级别，日志位置。

**日志级别：** 

`debug > info > notice > warn > error > crit > alert > emerg`

语法格式:

```text
access_log path [format_name [buffer=size] [gzip[=level]] [flush=time] [if=condition]];
# 示例
# access_log /var/logs/nginx-access.log compression buffer=32k;
```

### 1.3 open\_log\_file\_cache

open\_log\_file\_cache来设置日志文件缓存\(默认是off\)。

```text
# 语法格式: 
open_log_file_cache max=N [inactive=time] [min_uses=N] [valid=time];

# 默认值: open_log_file_cache off;
# 作用域: http, server, location
# 示例
# open_log_file_cache max=1000 inactive=20s valid=1m min_uses=2;
```



* max:设置缓存中的最大文件描述符数量，如果缓存被占满，采用LRU算法将描述符关闭。
* inactive:设置存活时间，默认是10s
* min\_uses:设置在inactive时间段内，日志文件最少使用多少次后，该日志文件描述符记入缓存中，默认是1次
* valid:设置检查频率，默认60s
* off：禁用缓存

### 1.4 日志中常用的全局变量

> * `$remote_addr`, `$http_x_forwarded_for` 记录客户端IP地址
> * `$remote_user`记录客户端用户名称
> * `$request`记录请求的URL和HTTP协议\(GET,POST,DEL,等\)
> * `$status`记录请求状态
> * `$body_bytes_sent`发送给客户端的字节数，不包括响应头的大小； 该变量与Apache模块mod\_log\_config里的“%B”参数兼容。
> * `$bytes_sent`发送给客户端的总字节数。
> * `$connection`连接的序列号。
> * `$connection_requests` 当前通过一个连接获得的请求数量。
> * `$msec` 日志写入时间。单位为秒，精度是毫秒。
> * `$pipe`如果请求是通过HTTP流水线\(pipelined\)发送，pipe值为“p”，否则为“.”。
> * `$http_referer` 记录从哪个页面链接访问过来的
> * `$http_user_agent`记录客户端浏览器相关信息
> * `$request_length`请求的长度（包括请求行，请求头和请求正文）。
> * `$request_time` 请求处理时间，单位为秒，精度毫秒； 从读入客户端的第一个字节开始，直到把最后一个字符发送给客户端后进行日志写入为止。
> * `$time_iso8601 ISO8601`标准格式下的本地时间。
> * `$time_local`通用日志格式下的本地时间。

## 二、nginx日志调试技巧

### 2.1 仅记录固定 IP 的错误

当你设置日志级别成 debug，如果你在调试一个在线的高流量网站的话，你的错误日志可能会记录每个请求的很多消息，这样会变得毫无意义。

在`events{...}`中配置如下内容，可以使 Nginx 记录仅仅来自于你的 IP 的错误日志。

```text
events {
        debug_connection 1.2.3.4;
}
```

### 2.2 调试 nginx rewrite 规则

调试rewrite规则时，如果规则写错只会看见一个404页面，可以在配置文件中开启nginx rewrite日志，进行调试。

```text
server {
        error_log    /var/logs/nginx/example.com.error.log;
        rewrite_log on;
}
```

`rewrite_log on` 开启后，它将发送所有的 rewrite 相关的日志信息到 error\_log 文件中，使用 \[notice\] 级别。随后就可以在error\_log 查看rewrite信息了。

### 2.3 使用location记录指定URL的日志

```text
server {
    error_log    /var/logs/nginx/example.com.error.log;
    location /static/ {
        error_log /var/logs/nginx/static-error.log debug; 
    }         
}
```

配置以上配置后，/static/ 相关的日志会被单独记录在static-error.log文件中。

## 三、常用示例

### main格式

```text
log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"'
                       '$upstream_addr $upstream_response_time $request_time ';
access_log  logs/access.log  main;
```

### json格式

```text
log_format logstash_json '{"@timestamp":"$time_iso8601",'
       '"host": "$server_addr",'
       '"client": "$remote_addr",'
       '"size": $body_bytes_sent,'
       '"responsetime": $request_time,'
       '"domain": "$host",'
       '"url":"$request_uri",'
       '"referer": "$http_referer",'
       '"agent": "$http_user_agent",'
       '"status":"$status",'
       '"x_forwarded_for":"$http_x_forwarded_for"}';
```



