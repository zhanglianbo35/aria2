# 第一章节  安装并配置  
## 安装aria2  
```bash
sudo apt-get install aria2  
```



## 安装完配置aria2  
```bash
sudo mkdir /etc/aria2      
sudo touch /etc/aria2/aria2.session      
sudo chmod 777 /etc/aria2/aria2.session    #权限设置aria2.session可读写执行 
```




## 将自定义配置信息写入配置文件 /etc/aria2/aria2.conf   
```bash
sudo -u ${HOME##*\/} bash -c 'cat <<EOT > /etc/aria2/aria2.conf  
#＝＝文件保存目录自行修改，请使用绝对路径  
dir=${HOME}/Downloads     
disable-ipv6=true 
#打开rpc的目的是为了给web管理端用  
enable-rpc=true   
rpc-allow-origin-all=true  
rpc-listen-all=true  
#rpc-listen-port=6800  
continue=true  
input-file=/etc/aria2/aria2.session  
save-session=/etc/aria2/aria2.session  
max-concurrent-downloads=10  
EOT'  
```




# 启动aria2，测试是否运行成功，无错误  
```bash
#sudo aria2c --conf-path=/etc/aria2/aria2.conf  
```


#  如果没有提示错误，按ctrl+c停止运行命令，转为后台运行： 
```bash
sudo aria2c --conf-path=/etc/aria2/aria2.conf -D  
```



# 把aria2做成服务启动  
```bash
sudo bash -c 'cat <<EOT >  /etc/init.d/aria2c  
#!/bin/sh  
###BEGIN INIT INFO  
#Provides: aria2  
#Required-Start: $remote_fs $network  
#Required-Stop: $remote_fs $network  
#Default-Start: 2 3 4 5  
#Default-Stop: 0 1 6  
#Short-Description: Aria2 Downloader  
###END INIT INFO  


case "$1" in  
start)  
 echo -n "已开启Aria2c"  
 sudo -u \${HOME##*\/} aria2c --conf-path=/etc/aria2/aria2.conf -D  
;;  

stop)  
 echo -n "已关闭Aria2c"  
 killall aria2c  
;;  

restart)  
 killall aria2c  
 sudo -u \${HOME##*\/} aria2c --conf-path=/etc/aria2/aria2.conf -D  
;;  

esac  

exit  
EOT'  
```




## 保存文件把权限给为755：  
```bash
sudo chmod 755 /etc/init.d/aria2c  
```



## 测试Aria2服务是否可以正常启动：  
```bash
sudo service aria2c start  
```


### *如果只显示“开启Aria2c”，没有其他错误提示的话就说明成功了。*  

## 添加Aria2c服务到开机启动  
```bash
sudo update-rc.d aria2c defaults 
```

 

## Aria2c服务命令使用说明：  
```bash
sudo service aria2c start    #启动Aria2c 
sudo service aria2c stop     #关闭Aria2c 
sudo service aria2c restart  #重启Aria2c
```


*Reference from: http://www.nixonli.com/linux/ubuntu/17040.html*  



# 第二章节  下载管理和使用  
配置web下载界面  
Aria2是没有界面的，但是可以开启并设置web界面管理  
这里主要是chrome插件  

## YAAW下载管理器  
基本配置 ：  
最小监视: 1 Minute  
JSON-RPC 链接:  http://localhost:6800/jsonrpc  

### 更多内容，详见[这里](https://www.jianshu.com/p/b2649d073741 "这里")      

