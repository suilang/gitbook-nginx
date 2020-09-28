# 简介

Nginx因为它的稳定性、丰富的模块库、灵活的配置和低系统资源的消耗而闻名。

### nginx可以提供的服务

1. web 服务.
2. 负载均衡 （反向代理）
3. web cache（web 缓存）

### nginx 的优点

1. 高并发。静态小文件
2. 占用资源少。2万并发、10个线程，内存消耗几百M。
3. 功能种类比较多。web,cache,proxy。每一个功能都不是特别强。
4. 支持epoll模型，使得nginx可以支持高并发。
5. nginx 配合动态服务和Apache有区别。（FASTCGI 接口）
6. 利用nginx可以对IP限速，可以限制连接数。
7. 配置简单，更灵活。

### nginx应用场合

1. 静态服务器。（图片，视频服务）另一个lighttpd。并发几万，html，js，css，flv，jpg，gif等。
2. 动态服务，nginx——fastcgi 的方式运行PHP，jsp。（PHP并发在500-1500，MySQL 并发在300-1500）。
3. 反向代理，负载均衡。日pv2000W以下，都可以直接用nginx做代理。
4. 缓存服务。类似 SQUID,VARNISH。

