# docker-compose
#### 1 什么是docker-compose
个人的理解是 ： 如果docker是一条linux命令， docker-compose就是shell脚本  
可以批量执行容器操作， 管理多个容器；  
例如：解决容器间的嵌套依赖；  

官方compose 文档： https://docs.docker.com/compose/  

docker-compose --help 命令很多，先上手做一个  

#### 2 docker compose 启动单个容器  
工程文件夹docker-compose-single
1、任意文件目录，创建docker-compose.yml配置文件  
2、将一个简单的REST springboot工程jar包移到目录下， 构建一个Dockerfile
3、在docker-compose文件中 将springboot的8080端口与宿主机9000端口绑定
4、docker-compose up (可以添加-d参数后台启动)
5、http://127.0.0.1:9000/user 访问成功


#### 3 docker compose 常用命令
~~~~
docker-compose ps       # 列出所有运行容器
docker-compose logs     # 服务日志输出
docker-compose port xxx port # 输出某服务某端口绑定的公共端口
docker-compose build    #构建/重新构建服务
docker-compose start xxx   # 启动容器
docker-compose stop xxx
docker-compose up (-d)  # 构建、启动容器 -d后台执行
docker-compose kill xxx  # 发送SIGKILL 执行停止xx服务的容器
docker-compose run xxx xxbash  # 在某服务商执行一个bash命令
~~~~

#### 4 docker-compose.yml属性
~~~~
version: # 指定 docker-compose.yml 文件的写法格式

services: # 多个容器集合

image: #指定服务要使用的镜像
image: java

build: # 构建时，compose利用他自动构建镜像，可以是一个路径/对象，也可以指定Dockerfile参数
#例如：
build: ./dir
----------
build:
    context: ./dir
    dockerfile: Dockerfile
    args: 
        buildno: 1


command: # 覆盖容器后启动执行默认命令
command: bundle exec thin -p 3000

command: [bundle, exec, thin, -p, 3000]


ports: # 对外暴露接口，和expose对应 "宿主机端口：容器暴露端口"

dns: # 配置dns服务器 
dns: 8.8.8.8
dns: 
    - 8.8.8.8
    - 9.9.9.9

~~~~