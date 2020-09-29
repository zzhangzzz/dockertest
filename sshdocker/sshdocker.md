# 练习构建一个带ssh功能的dockerbox
主要记录遇到的几个问题：  
#### 1 镜像的问题
本来是用163的镜像，但是一直有校验的问题，后来换成清华的镜像 finish
#### 2 本地开发的问题
重要的一步 RUN apt-get update 未执行 导致无法下载
#### 3 本地key不存在
没有复制宿主机的key到docker build的文件目录
