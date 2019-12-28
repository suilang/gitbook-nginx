# 一个简单的conf示例



```text
# nginx.conf

worker_processes  1;
events {
    worker_connections  1024;
}
http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;
    #blog lb by oldboy at 201303
    upstream blog_real_servers {
       server   10.0.0.9:80 weight=1 max_fails=1 fail_timeout=10s;
       server   10.0.0.10:80 weight=1 max_fails=2 fail_timeout=20s;
    }
    server {
       listen       80;
       server_name  blog.etiantian.org;
       location / {
        proxy_pass http://blog_real_servers;
        include proxy.conf;
       }
    }
}
```

```text
 # proxy.conf 
 
 proxy_set_header Host $host;
 proxy_set_header X-Forwarded-For $remote_addr;
 proxy_connect_timeout 90;        
 proxy_send_timeout 90;
 proxy_read_timeout 90;
 proxy_buffer_size 4k;
 proxy_buffers 4 32k;
 proxy_busy_buffers_size 64k; proxy_temp_file_write_size 64k;
```

