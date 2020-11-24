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

​		



