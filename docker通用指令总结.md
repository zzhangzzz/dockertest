# docker 常用命令总结
### 总览
~~~~
ADD
COPY         复制文本
ENV
EXPOSE
FROM         指定镜像
RUN          执行shell 命令
STOPSIGNAL
USER
VOLUME
WORKDIR
~~~~

#### 1 FROM 指定镜像
~~~~
FROM [--platform=xx] <image> [AS name]
FROM [--platform=xx] <image>[:tagx] [AS name]
FROM [--platform=xx] <image>[:digest] [AS name]

--platform 是指定镜像平台 例如linux/amd64 、 windows/amd64等
tag、digest都是可选项
name 自定义
~~~~  

范例  
~~~~
# 起源镜像，相当于 net object
FROM scratch 
FROM ubuntu
FROM ubuntu:tagA
FROM ubuntu:slim as dockeraas
~~~~  

LABEL 来指定镜像的基础信息 格式是
~~~~
LABEL key=value key=value
~~~~  
例如  
~~~~
LABEL "com.example.vendor"="ACM INcorporated"
LABEL version="1.0"
LABEL multi.label1="value1" multi2="value2" \
        multi.label3="value3"


docker inspect命令可以查看label

ps：MAINTAINER 命令已被LABEL取代
~~~~  

#### 2 RUN 执行shell 命令
~~~~
#shell格式
RUN xxxx

#exec格式
RUN ["可执行文件", "参数1", "参数2"]
~~~~  

范例  
~~~~
RUN echo '<h1> Hello,docker,world</h1>' > /usr/share/html/index.html
RUN ["/bin/bash", "-c", "echo hello world"]
RUN yum -y install nginx \
    rm -rf /usr/share/nginx/html/* && echo "<h1> docker nginx</h1>" > /usr/share/html/index.html
~~~~  

#### 3 COPY  复制文本 复制本地主机到容器中（从Dockerfile所在目录的相对路径）
格式  
~~~~
COPY src .. res
COPY ["src1", "src2" ... "目标路径"]
~~~~  
范例  
~~~~
COPY home/* /mydir/
COPY hom?.txt /mydir/
~~~~

#### 4 ADD 复制和解包文件 实际是增强的COPY 不仅支持copy 还直接解压缩  
~~~~
# 如果src是目录，只复制目录中的内容，而非目录本身
# 如果src是本地打包或压缩文件，如gzip, bzip2 ，xz ，将解包
# 如果src是一个 URL ，下载后的文件权限自动设置为 600
ADD [--chown=user:group] src  res
~~~~  
范例  
~~~~
ADD test relativeDir/  # 等于 add test to WORKDIR/relativeDir
ADD test /absoluteDir  
ADD --chown=55:group files* /dir
ADD --chown=1 files* /dir
ADD ubuntu-xenial-core-clouding-amd64-root.tar.gz /
~~~~

#### 5 CMD 容器启动命令 
指定容器启动时默认执行的命令， 每个dockerfile只能有一条CMD命令， 多条只有最后一条执行  
如果用户启动容器指定了运行命令，如 ：docker run xxx /bin/bash 则bin/bash会覆盖 CMD  
格式  
~~~~
CMD ["executable", "parms1", "param2"]
CMD command param1 param2
CMD ["param1", "param2"]
~~~~  
范例  
~~~~
CMD ["nginx", "-g", "daemon off"]
CMD [ "/run.sh" ]
~~~~



### 练习 
~~~~
FROM ubutun:18.04
LABEL user=zzzhangzz
RUn apt update  && apt -y install curl \
    && rm -rf /var/lib/apt/lists/*
CMD ["curl", "-s", "https://ip.cn"]
~~~~


#### 6 ENTRYPOINT 入口点，类似CMD，主要是配置容器启动后的命令及参数
不能被docker run的参数覆盖，而是追加的效果  
如果docker run 没有额外参数，但是Dockerfile的CMD 中有，CMD的内容会作为ENTRYPOINT的参数  
可以通过 docker run --entrypoint 在运行中替换

一个dockerfile 有多个ENTRYPOINT 只有最后一个有效  

格式
~~~~
ENTRYPOINT ["executable", "parma1", "param2"]
ENTRYPOINT command param1 param2
~~~~  

范例  
~~~~
FROM ubutun:18.04
LABEL user=zzzhangzz
RUn apt update  && apt -y install curl \
    && rm -rf /var/lib/apt/lists/*
ENTRYPOINT ["curl", "-s", "https://ip.cn"]
~~~~  


#### 7 ENV 指定环境变量 会被后续RUN指令使用
格式  
~~~~
ENV key value 
ENV key=value key2=value2 ...
~~~~  

范例  
~~~~
ENV Version=1.0 DEBUG=on Name="test docker"

ENV PG_VERSION 9.3.4
ENV PATH /usr/local/postgres-$PG_MAJOR/bin:$PATH
~~~~

#### 8 ARG 构建参数
如果ARG 和EVN 同名 EVN会覆盖ARG ，并且容器不会存在这些环境变量  
范例  
~~~~
FROM busybox
ARG user1=userss
ARG buildno=1

FROM busybox
ARG SETTINGS
RUN ./run/setup $SETTINGS
~~~~

#### 9 VOLUME 挂载点
在容器中创建一个可以从本地主机 / 其他容器挂载的 挂载点， 用来存放数据库和要记录的数据，一般是宿主机的目录   
~~~~
VOLUME ["/data1", "/data2/sss/tmp"]
~~~~


#### 10 EXPOSE 暴露接口
DOCKER服务端容器暴露的端口号 注意：：  
只是声明容器打算用什么端口，不会再宿主机进行端口映射  
所以，容器在启动时需要通过 -P 或 -p docker主机分配一个端口到指定暴露的端口，才能使用  
~~~~
EXPOSE 80 443
~~~~

#### 11 WORKDIR  指定工作目录
是容器运行后的，进入容器的默认目录， 为后续RUN、CMD 、ENTRYPOINT指令配置工作目录   

注意 如果是多条WORKDIR命令， 会基于之前的命令  
~~~~
WORKDIR /a
WORKDIR b
WORKDIR c
RUN pwd  # 此时路径是 /a/b/c
~~~~

#### 12 HEALTHCHECK 健康检查
格式  
~~~~
HEALTHCHECK --interval=间隔 #两次之间的时间间隔 默认30s
--timeout=xx #超时时间 默认30s
--retries=x # 重试次数 默认3次
~~~~

范例  
~~~~
FROM nginx
RUN apt-get update && apt-get install -y curl && rm -fr /var/lib/apt/lists/*
HEALTHCHECK --interval=5s --timeout=3s \
CMD curl -fs http://localhost/
~~~~  


#### 13 STOPSIGNAL 退出容器信号