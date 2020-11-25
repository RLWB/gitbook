## 前端项目部署

### 一、准备工作

* 购买服务器

  * 服务器提供商选择，阿里云，腾讯云，华为云，亚马逊云，Azure，谷歌云
  * 合理选择服务器的配置

* 远程连接服务器的工具和上传项目文件的工具

  * xshell
  * filezilla

* Nginx基本安装和使用方法，依照Ubuntu 18系统为例：

  ```shell
  sudo apt-get update # 更新软件列表
  
  sudo apt-get upgrade -y # 对比各软件版本并更新
  
  sudo apt-get install nginx -y # 安装nginx服务器
  
  service nginx start # 启动nginx服务器
  ```

  * nginx常用功能：

    1、HTTP代理，反向代理

    ![img](https://www.runoob.com/wp-content/uploads/2018/08/1535725078-5993-20160202133724350-1807373891.jpg)

    2、负载均衡

    3、web缓存  

### 二、修改nginx配置

* nginx配置文件

  ```nginx
  ########### 每个指令必须有分号结束。#################
  #user administrator administrators;  #配置用户或者组，默认为nobody nobody。
  #worker_processes 2;  #允许生成的进程数，默认为1
  #pid /nginx/pid/nginx.pid;   #指定nginx进程运行文件存放地址
  error_log log/error.log debug;  #制定日志路径，级别。这个设置可以放入全局块，http块，server块，级别以此为：debug|info|notice|warn|error|crit|alert|emerg
  events {
      accept_mutex on;   #设置网路连接序列化，防止惊群现象发生，默认为on
      multi_accept on;  #设置一个进程是否同时接受多个网络连接，默认为off
      #use epoll;      #事件驱动模型，select|poll|kqueue|epoll|resig|/dev/poll|eventport
      worker_connections  1024;    #最大连接数，默认为512
  }
  http {
      include       mime.types;   #文件扩展名与文件类型映射表
      default_type  application/octet-stream; #默认文件类型，默认为text/plain
      #access_log off; #取消服务日志    
      log_format myFormat '$remote_addr–$remote_user [$time_local] $request $status $body_bytes_sent $http_referer $http_user_agent $http_x_forwarded_for'; #自定义格式
      access_log log/access.log myFormat;  #combined为日志格式的默认值
      sendfile on;   #允许sendfile方式传输文件，默认为off，可以在http块，server块，location块。
      sendfile_max_chunk 100k;  #每个进程每次调用传输数量不能大于设定的值，默认为0，即不设上限。
      keepalive_timeout 65;  #连接超时时间，默认为75s，可以在http，server，location块。
  
      upstream mysvr {   
        server 127.0.0.1:7878;
        server 192.168.10.121:3333 backup;  #热备
      }
      error_page 404 https://www.baidu.com; #错误页
      server {
          keepalive_requests 120; #单连接请求上限次数。
          listen       4545;   #监听端口
          server_name  127.0.0.1;   #监听地址       
          location  ~*^.+$ {       #请求的url过滤，正则匹配，~为区分大小写，~*为不区分大小写。
             #root path;  #根目录
             #index vv.txt;  #设置默认页
             proxy_pass  http://mysvr;  #请求转向mysvr 定义的服务器列表
             deny 127.0.0.1;  #拒绝的ip
             allow 172.18.5.54; #允许的ip           
          } 
      }
  }
  ```

  



