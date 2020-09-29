# 继承自上一次创建的sshd镜像
FROM sshd:dockerfile

# 环境变量 操作非交互式
ENV DEBIAN_FRONTEND noninteractive

RUN apt-get -yq install apache2&&\
rm -rf /var/lib/apt/lists/*


# 更改时区设置
RUN echo "Asia/Shanghai" > /etc/timezone && \
dpkg-reconfigure -f noninteractive tzdata

ADD run.sh /run.sh
RUN chmod 755 /*.sh

# 添加一个范例web 删除默认apache文件夹下的文件，将示例用软连接链接到/var/www/html下面
RUN mkdir -p /var/lock/apache2 && mkdir -p /app && rm -fr /var/www/html && ln -s /app /var/www/html
COPY sample/ /app

#设置apache 变量  也可以在容器启动时 用-e参数替代
ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/log/apache2
ENV APACHE_PID_FILE /var/run/apache2.pid
ENV APACHE_RUN_DIR /var/run/apache2
ENV APACHE_LOCK_DIR /var/lock/apache2
ENV APACHE_SERVERADMIN admin@localhost
ENV APACHE_SERVERNAME localhost
ENV APACHE_SERVERALIAS docker.localhost
ENV APACHE_DOCUMENTROOT /var/www

EXPOSE 80
WORKDIR /app

CMD ["/run.sh"]
