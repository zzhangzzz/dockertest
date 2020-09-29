# from 指定基础镜像 scratch表示不基于任何基础镜像
FROM scratch
# copy 表示复制本地文件到指定目录 本地文件目录是相对Dockerfile文件所在的目录 不是系统根目录
COPY hello / 
# 指定容器启动时默认执行的命令 一个dockerfile只有最后一条CMD有效 其他都会被忽略
CMD ["/hello"]

