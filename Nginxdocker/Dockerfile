FROM sshd:dockerfile

# 安装nginx 设置非 daemon启动
RUN \
apt-get install -y nginx && \
rm -rf /var/lib/apt/lists/* && \
echo "\ndaemon off;" >> /etc/nginx/nginx.conf && \
chown -R www-data:www-data /var/lib/nginx

RUN echo "Asia/Shanghai" > /etc/timezone && dpkg-reconfigure -f noninteractive tzdata

ADD run.sh /run.sh
RUN chmod 755 /*.sh

# 定义可以被挂载的目录 分别是 虚拟主机、证书、配置、日志
VOLUME [ "/etc/nginx/sites-enabled", "/etc/nginx/certs", "/etc/nginx/conf.d", "/var/log/nginx" ]

# 工作目录
WORKDIR /ext/nginx

CMD [ "/run.sh" ]

EXPOSE 80
EXPOSE 443
