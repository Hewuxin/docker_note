Docker开发部署流程： 本地开发、测试----> 打包为Docker镜像(可以理解为软件安装包) ---->各种服务器上只需要一个命令部署好

$\color{green}优点：确保不同机器上跑的都是一致的运行环境，不会出现一个机器跑正常，另一个跑存在问题的情况$

## 用途

- 应用分发、部署，方便传播给他人安装
- 快速安装测试软件
- 多个版本软件共存，不污染系统

## docker快速安装软件

docker 官方镜像仓库查找 https://hub.docker.com

### 1. docker安装jenkins

1. 搜索jenkins镜像

   `docker search jenkins`

2. 拉取jenkins镜像

   `docker pull jenkins/jenkins`

3. 启动jenkins容器

   `docker run -d -uroot -p 9095:8000 -p 50000:50000 --name jenkins_demo -v /Users/heyuyang/Study/jenkins_home:/var/jenkins_home jenkins/jenkins`

   - `-d` 后台运行，并返回容器ID
   - `-uroot` 使用root身份进入容器， 避免容器内执行某些命令时出现 权限问题
   - `-p 9095:8080` 将容器内8080端口映射到宿主机905端口， 这个是访问jenkins的端口
   - `-p 50000:50000` 将容器内50000端口映射到宿主机50000端口
   - `-name jenkins_demo` 设置的容器名称
   - `-v /Users/heyuyang/Study/jenkins_home:/var/jenkins_home`
     - `/Users/heyuyang/Study/jenkins_home`目录为硬盘上的目录
     - `/var/jenkins_home`为jenkins工作目录,将硬盘上的`.../jenkins_home`挂在到这个位置，方便后续更新镜像后继续使用原来的工作目录
   - `jenkins/jenkins`镜像的名称或镜像ID

4. 查看容器日志，获取初始密码

   `docker logs jenkins_demo`

   初始密码存放在jenkins容器的`/var/jenkins_home/secrets/initialAdminPassword`文件中

5. 访问jenkins
   - 在浏览器中输入`http://serverIp:port`访问jenkins
   - serverIP为docker宿主机的ip
   - port为宿主机映射的端口
   - 本地为`http://localhost:9095`，并输入4中获取的密码

6. 插件下载

   因为网络问题，需要将插件源设置为国内的，这样才可以安装插件。

   - 进入宿主机`/Users/heyuyang/Study/jenkins_home`目录,编辑`hudson.model.UpdateCenter.xml`中的url为` https://mirrors.tuna.tsinghua.edu.cn/jenkins/updates/update-center.json`

   - 重启容器

     `docker restart jenkins_demo`

### 2. docker安装运行redis

`docker run -d -p 6379:6379 --name redis redis:latest`

- `docker run` 表示 docker运行一个软件

- `-d` 表示在后台运行
- `-p 6379：6379`  表示 将容器的里面6379端口暴露到宿主机6379端口
- `-name redis` 表示
- `redis:latest` 表示拉取的软件源

## docker基本命令

参考 https://docs.docker.com/engine/reference/commandline/run/

### 1. 查看docker相关信息

1. 查看版本信息

​	`docker version`

2. 查看docker基本信息

​	`docker info`

3.  使用 Docker Compose 来管理多个容器的应用

   `docker-compose`

4. 查看docker帮助信息

   `docker --help`

### 2.镜像(images)相关命令

1. 列出本地镜像

   `docker images`

2. 在 docker hub 上搜索镜像

   `docker search <image_name>`

3. 从 docker hub 上下载指定镜像

   `docker pull <image_name>`

4. 删除指定的本地镜像

   `docker rmi <image_name>`

### 3. 容器(containers)相关命令

1. 列出正在运行的容器

   `docker ps`

2. 列出所有容器(包括已经停止的)

   `docker ps -a`

3. 启动一个新的容器

   `docker run <image_name>`

4. 启动已停止的容器

   `docker start <container_id>`

5. 停止运行中的容器

   `docker stop <container_id>`

6. 重启容器

   `docker restart <container_id>`

7. 删除容器

   `docker rm <container_id>`

8. 查看容器的日志

   `docker logs <container_id>`

### 4. 容器操作

1. 在运行中的容器内执行命令

   `docker exec -it <container_id> <command>`

2. 从本地系统复制文件到容器内

   `docker cp <local_file_path> <container_id>:<container_file_path>`

3. 从容器内复制文件到本地系统

   `docker cp <container_id>:<container_file_path>  <local_file_path>`

### 5. 网络相关命令

1. 列出docker网络

   `docker network ls`

2. 查看特定网络的详细信息

   `docker network inspect <network_name>`

