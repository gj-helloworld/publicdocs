## Links
[[README](../README.md)]
[[About Nginx](<../doc/about-nginx.md>)]

# Nginx

## 什么是Nginx

Nginx，读音『engine X』，它主要是web引擎，主要服务HTTP协议，支持各种各样的HTTP的场景，比如反代、静态站点、负载均衡等。除了HTTP之外，Nginx还支持多种协议，如IMAP, POP3, STMP, TCP, UDP。

### 参考

> [官网](https://www.nginx.com/resources/glossary/nginx/)

> [wikipedia](https://zh.wikipedia.org/wiki/Nginx)

## 我司使用Nginx的场景

我司只考虑使用HTTP的反代场景，即，把多个系统，集中到一个nginx站点中，如：
- http://192.168.21.229/ecimp-1/
- http://192.168.21.229/ecimp-2/
- ...
- http://192.168.21.229/ecimp-n/

上述实例是运行在局域网中的多个ecimp实例
- http://192.168.21.100:8080/
    - 被反代到：http://192.168.21.229/ecimp-1/
- http://192.168.21.101:8080/
    - 被反代到：http://192.168.21.229/ecimp-2/
- ...
- http://192.168.21.101:8081/
    - 被反代到：http://192.168.21.229/ecimp-n/

经过nginx的反代，所有的站点就因此被反代到同一个nginx的web服务上，该nginx可以部署到地面系统上。
- http://192.168.21.229/.../

## Nginx的安装

nginx的安装，只需要三步
- [下载](http://nginx.org/en/download.html)
- 解压到随意一个目录
- 运行
    - start nginx
    - nginx -s stop
    - nginx -s quit
    - [更多](http://nginx.org/en/docs/windows.html)

### 参考

> [nginx for Windows](http://nginx.org/en/docs/windows.html)

## Nginx反代相关配置

反代相关的配置相对比较简单，配置一目了然。

nginx.conf文件如下，该文件在nginx运行目录的conf目录之下

```
events {
    worker_connections  1024;
}

http {
    server {
        listen       80;
        location /ecimp-01/ {
            proxy_pass http://localhost:8080/;
            proxy_cookie_path / /ecimp-01;
        }
        location /ecimp-02/ {
            proxy_pass http://192.168.21.38:8080/;
            proxy_cookie_path / /ecimp-02;
        }
    }
}
```

或者用上述配置代码，直接替换目标nginx的配置，修改相关proxy_pass的地址，然后重启nginx，就可以获得nginx服务
- nginx -s stop
- start nginx

