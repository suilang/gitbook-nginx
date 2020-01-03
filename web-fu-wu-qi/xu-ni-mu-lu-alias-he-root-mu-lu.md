# 虚拟目录alias和root目录

nginx是通过alias设置虚拟目录，在nginx的配置中，

### alias目录和root目录的区别

1. alias指定的目录是准确的，即location匹配访问的path目录下的文件直接是在alias目录下查找的；
2. root指定的目录是location匹配访问的path目录的上一级目录,这个path目录一定要是真实存在root指定目录下的；
3. 使用alias标签的目录块中不能使用rewrite的break（具体原因不明）；另外，alias指定的目录后面必须要加上"/"符号！！
4. alias虚拟目录配置中，location匹配的path目录如果后面不带"/"，那么访问的url地址中这个path目录后面加不加"/"不影响访问，访问时它会自动加上"/"； 但是如果location匹配的path目录后面加上"/"，那么访问的url地址中这个path目录必须要加上"/"，访问时它不会自动加上"/"。如果不加上"/"，访问就会失败！ 
5. root目录配置中，location匹配的path目录后面带不带"/"，都不会影响访问。

#### 示例一

location /demo/ {  
      alias /home/folder/demo/;  
}

在上面alias虚拟目录配置下，访问http://www.domain.com/demo/a.html实际指定的是/home/folder/demo/a.html。

> 注意：alias指定的目录后面必须要加上"/"，即/home/folder/demo/不能改成/home/folder/demo

上面的配置也可以改成root目录配置，如下，

location /huan/ {  
       root /home/folder/;  
}

这样nginx就会去/home/folder/demo下寻找http://www.domain.com/demo的访问资源，两者配置后的访问效果是一样的！



#### 示例二

alias设置的目录名和location匹配访问的path目录名不一致

```text
location /image/ {
      alias /home/folder/demo/;
}
```

访问http://www.domain.com/image的时候就会去/home/folder/demo/下寻找访问资源。  
这样的话，不能直接改成root目录配置。  
如果非要改成root目录配置，就只能在/home/www下将html-&gt;web（做软连接，即快捷方式），如下：

```text
location /image/ {
     root /home/floder/;
}
ln -s /home/folder/image /home/folder/demo    
```

  
  


