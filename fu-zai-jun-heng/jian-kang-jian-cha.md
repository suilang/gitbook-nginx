# 健康检查

Nginx提供了health\_check语句来提供负载（upstream）时的键康检查机制（注意：此语句需要设置在location上下文中）。

支持的参数有：

* interval=time：设置两次健康检查之间的间隔值，默认为5秒
* fails=number：设置将服务器视为不健康的连续检查次数，默认为1次
* passes=number：设置一个服务器被视为健康的连续检查次数，默认为1次
* uri=uri：定义健康检查的请求URI，默认为”/“
* match=name：指定匹配配置块的名字，用于测试响应是否通过健康检测。默认为测试返回状态码为2xx和3xx

一个简单的设置如下，将使用默认值：

```text
location / {
    proxy_pass http://backend;
    health_check;
}
```

可以专门定义一个API用于健康检查：/api/health\_check，并只返回HTTP状态码为200。并设置两次检查之间的间隔值为1秒。这样，health\_check语句的配置如下：

```text
health_check uri="/api/health_check" interval;
```

匹配match的方法

```text
http {
    server {
    ...
        location / {
            proxy_pass http://backend;
            health_check match=welcome;
        }
    }

    match welcome {
        status 200;
        header Content-Type = text/html;
        body ~ "Welcome to nginx!";
    }
}
```

match 例子举例

* `status 200;`: status 等于 200
* `status ! 500;`: status 不是 500
* `status 200 204;`: status 是 200 或 204
* `status ! 301 302;`: status 不是301或302。
* `status 200-399;`: status 在 200 到 399之间。
* `status ! 400-599;`: status 不在 400 到 599之间。
* `status 301-303 307;`: status 是 301, 302, 303, 或 307。
* `header Content-Type = text/html;`: “Content-Type” 的值是 text/html。
* `header Content-Type != text/html;`: “Content-Type” 不是 text/html。
* `header Connection ~ close;`: “Connection” 包含 close。
* `header Connection !~ close;`: “Connection” 不包含 close。
* `header Host;`: 请求头包含 “Host”。
* `header ! X-Accel-Redirect;`: 请求头不包含 “X-Accel-Redirect”。
* `body ~ "Welcome to nginx!";`: body 包含 “Welcome to nginx!”。
* `body !~ "Welcome to nginx!";`: body 不包含 “Welcome to nginx!”。



