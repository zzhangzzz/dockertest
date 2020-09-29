# 继承镜像
FROM ubuntu:14.04

# MAINTAINER zzzhang

# 替换为国内163的源
RUN echo "deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ trusty main restricted universe multiverse" > /etc/apt/sources.list
RUN echo "deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ trusty-updates main restricted universe multiverse" >> /etc/apt/sources.list
RUN echo "deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ trusty-security main restricted universe multiverse" >> /etc/apt/sources.list
RUN echo "deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ trusty-backports main restricted universe multiverse" >> /etc/apt/sources.list
#RUN echo "deb https://mirror.tuna.tsinghua.edu.cn/help/ubuntu/ trusty-backports main restricted universe multiverse" >> /etc/apt/sources.list


# 这个步骤一开始忘记了 不更新会导致无法找到软件源而下面的下载失败
RUN apt-get update 

# 安装ssh 服务
RUN apt-get install -y openssh-server
RUN mkdir -p /var/run/sshd
RUN mkdir -p /root/.ssh

# 取消pam限制  pam是限制登陆失败的一个东西
RUN sed -ri 's/session  required pam_loginuid.so/#session required pam_loginuid.so/g' /etc/pam.d/sshd

# 复制配置文件到对应位置 给脚本执行权限
ADD authorized_keys /root/.ssh/authroized_keys
ADD run.sh /run.sh
RUN chmod 755 /run.sh

# 开放端口22
EXPOSE 22

# 自启动命令
CMD ["/run.sh"]

