# 配置文件解读

nginx配置文件主要分为四个部分：

```text
main{ 
#（全局设置）
    http{ 
    #服务器
        upstream{} #（负载均衡服务器设置：主要用于负载均衡和设置一系列的后端服务器）
        server{ 
            #（主机设置：主要用于指定主机和端口）
            location{} #（URL匹配特点位置的设置）
        }
    }
}
```

server继承main，location继承server，upstream即不会继承其他设置也不会被继承。


