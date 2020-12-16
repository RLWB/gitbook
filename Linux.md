常见命令
1.创建目录
    mkdir 目录名
 
2.创建文件
    touch 文件名
 
3.查看文件
    cat 文件名
 
4.删除
    rm -rf 文件或者目录名
 
5.复制
    cp 源文件 目标位置 
    cp -r  ./ys/ ./hys/             
    注: -r 复制目录
 
6.mv 剪切或改名         
    (1)剪切                
        mv 源文件 目标位置
        如: mv weixin ./software            
    (2)重命名               
        mv 源文件名 新名字
 
7.清屏
    clear 或者 Ctrl + l
 
8.强制终止
    ctrl + C
 
9. 上传
    命令:
        rz
    参数:
        -r --即文件传输中断会重传
        -y --表示文件已存在的时候会覆盖
10. 下载
    命令:
        sz
    例子: 将当前目录下的html文件夹，打包生成new.tar.gz
        tar -zcvf new.tar.gz html
        sz new.tar.gz
 

查看目录
1.    ls      显示目录下的内容
        1.1 ls -l  //长格式显示（缩略选项用一个减号，完整的选项是两个减号）
 
        1.2 ls -hl -h 人性化显示
 
        1.3 ls -a 显示所有文件
            文件名前面带.的是隐藏文件
  
2.    cd     切换所在目录
        cd      //回到登录用户的家目录
        cd /home //进入下一级目录
        cd /    //进入根目录
        cd -    //进入上一次操作目录
        cd ..   //进入上一级目录
        tab 键 可以对我们的目录和文件进行补全      
        相对路径
            参考当前所在目录，进行查询，如果使用相对路径，请查看好你的所在目录
 
        绝对路径
            从根目录开始一级一级查找，直接找到位置
 
 
3.     pwd 显示当前所在目录
 
4.     linux 常见目录（以下目录必须全部记录起来）
        /根目录
        /root       超级管理员的家目录
        /root/home  普通用户的家目录
        /bin        命令保存目录(普通用户的)
        /sbin       命令保存目录（超级管理员的）
        /boot       启动目录 启动相关文件内容
        /dev       设备文件保存目录
        /etc       配置文件保存目录
        /lib       函数库保存目录
        /mnt       系统挂载目录(推荐使用)
        /media      挂载目录
        /tmp        临时目录
        /proc       直接写入内存
        /usr        系统软件目录
        /var        系统相关文档内容
        /var/log    系统日志   
　　

　

解压

解压
    tar -xvf filename.tar
压缩
    tar -cvf filename.tar
 
其中zxvf含义分别如下:
z: 　　gzip  　　　　　　　　    压缩格式
x: 　  extract　　　　　　　　   解压
c:    create                  创建压缩文件
v:　　 verbose　　　　　　　　   详细信息
f: 　　file(file=archieve)　　 文件
 
注:从1.15版本开始tar就可以自动识别压缩的格式,故不需人为区分压缩格式就能正确解压
 

查看文件大小
1
2
3
du 目标目录 -sh
    -h, --human-readable  // 显示单位,以可读格式打印大小（例如1K 234M 2G）
    -s, --summarize       // 只显示一个合计值,仅显示每个参数的总计