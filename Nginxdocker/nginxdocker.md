# 包含nginx的docker
docker build -t nginx:sta
docker run -d -P nginx:sta

### 注意的问题
vscode自动保存问题  
nginx实际对外暴露接口问题

根据实际端口 127.0.0.1:port nginx页面即ok