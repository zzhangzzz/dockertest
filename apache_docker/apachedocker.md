# 创建apachedocker 中遇到的问题
按照自己做的顺序 & 查询和解决办法 希望给大家创建docker时提供帮助

构建命令
docker build -t apache:ubuntu .  
运行  
docker run -d -P apache:ubuntu 
-P参数映射需要开放的端口 22和80  

#### 按照dockerfile构建好 但是运行docker直接失败
通过docker ps -a 看到container的状态时exit    
docker ps 也没有容器正在运行
判断是容器的创建的问题  
仔细过了一遍dockerFile， 把copy命令跟 上面的RUN 命令写到一行去了  

中间又发现文件链接错误  

全部解决后重新build，再运行， 发现docker ps 成功启动了容器， 并且又一个本地端口 32452->tcp:80 确定启动成功

浏览器输入 127.0.0.1:32452  
成功输出sample/index.html的内容  
激动！