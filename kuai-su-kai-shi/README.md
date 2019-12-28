# 快速开始

## 一、安装nginx

### 在maxOS上安装nginx

1. 确保已经安装homebrew
2. 更新brew版本：`brew --update`
3. 安装nginx： `brew install nginx`

## 二、验证安装

```text
# 查看nginx配置文件目录
$ open /usr/local/etc/nginx/

# 启动nginx
$ nginx

# 查看版本
$ nginx -v

```

当安装成功并且启动nginx后，打开浏览器访问`localhost:8080`，就能看到nginx的欢迎页

## 三、关键文件、命令和目录

### nginx文件和目录

* /etc/nginx/：配置文件的默认根目录
* /etc/nginx/nginx.conf 默认的配置文件
* /etc/nginx/conf.d/ 包含默认的HTTP服务器配置文件，文件以.conf结尾，并且文件被

  /etc/nginx/nginx.conf文件内的顶层http模块所引用

* /var/log/nginx/ 默认的log日志位置，在这个目录下可以找到access.log 和error.log 文件

### nginx命令

* nginx -h 显示help菜单
* nginx -v 显示nginx版本
* nginx -V：显示nginx版本、构建信息和配置参数
* nginx -t：测试nginx配置
* nginx -T： 测试nginx配置并且打印有效的配置到命令窗
* nginx -s signal： 发送信号给nginx主进程，包含 stop、quit、reload和reopen。

| 信号量 | 用途 |
| :--- | :--- |
| stop | 立即结束nginx进程 |
| quit | 在完成当前任务后结束进程 |
| reload | 使用配置文件重启nginx进行 |
| reopen | 指示nginx重新打开日志文件 |

四、配置符号参考

| 容量符号 | 含义 |
| :--- | :--- |
| k,K | 千字节 |
| m,M | 兆字节 |

| 时间符号 | 含义 |
| :--- | :--- |
| ms | 毫秒 |
| s | 秒 |
| m | 分钟 |
| h | 小时 |
| d | 日 |
| w | 周 |
| M | 一个月, 30天 |
| y | 年, 365 天 |

