# Nginx

1. nginx目录结构
* conf/nginx.conf  ``nginx配置文件``
* html  
* logs
* sbin/nginx  ``二进制文件，用于启动、停止Nginx服务``

2. nginx命令
* 查看版本[sbin]  ``./nginx -v``
* 检查配置文件正确性[sbin]  ``./nginx -t``
* 启动nginx[sbin]  ``./nginx``
* 停止nginx[sbin]  ``./nginx -s stop``
* 启动后查看进程[sbin]  ``ps -ef | grep nginx``
* 重新加载配置文件  ``./nginx -s reload``

3. nginx配置环境变量

4. nginx配置文件结构
* 全局块  ``和Nginx运行相关的配置``
* events块  ``和网络连接相关的配置``
* http块  ``代理、缓存、日志记录、虚拟主机配置``
  * http全局块
  * Server块
    * Server全局块
    * location块

5. Nginx具体应用
* 部署静态资源
* 反向代理
```
location ^~ /api/ {
  rewrite ^/api/(.*)$ /$1 break;
  proxy_pass http://192.168.138.101:8080;
}
```
* 负载均衡
```yaml
upstream targetserver{
  server 192.168.138.101:8080;
  server 192.168.138.101:8081;
}

server{
  listen 8080;
  server_name localhost;
  location / {
    proxy_pass http://targetserver;
  }
}
```