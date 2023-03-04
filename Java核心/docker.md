开发环境：编写代码

测试环境：项目部署，编写测试单元

生产环境：正式运行



如果运行时环境没有进行事先约定，代码在不同环境上运行可能会出现“水土不服”的症状



docker：将代码和环境放在一个容器中，测试和运行直接在容器中使用

* 开源应用容器引擎
* 基于Go语言实现
* 开发者打包应用以及依赖包到一个轻量级、可移植的容器中，以此发布到任何流行版本的Linux上
* 容器使用沙箱机制，相互隔离，可以同时启动多个容器
* 性能开销低



docker是一种容器技术，主要解决软件跨环境迁移的问题



基于CentOS7安装docker

```shell
# 1、yum包更新到最新
yum update

# 2、安装需要的软件包，yum-util提供yum-config-manager功能，另外两个是devicemapper驱动依赖的包
yum install -y yum-utls device-mapper-persistent-data lvm2

# 3、设置yum源
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

# 4、安装docker，出现输入界面都按y
yum install -y docker-ce

# 5、查看docker版本，验证是否验证成功
docker -v
```



docker的架构图

<img src="C:/Users/%E4%BF%AE%E9%9B%AF%E5%A4%A9/AppData/Roaming/Typora/typora-user-images/image-20230304195937100.png" alt="image-20230304195937100" style="zoom:50%;" />

* 镜像（Image）：docker镜像相当于一个root文件系统。例如官方镜像Ubuntu：16.04就包含了一整套完整的Ubuntu16.04最小系统的root文件系统
* 容器（Container）：镜像和容器的关系，就像是面向对虾干程序设计中的类和对象一样，镜像是静态的定义，容器是镜像运行的实体。容器可有被创建、启动、停止、删除、暂停等
* 仓库（Repository）：仓库可以看做一个代码控制中心，用来保存镜像



配置docker镜像加速器

默认情况下，从dockerhub上下载docker镜像，速度很慢，一般会配置镜像加速器







```shell
# 1、打开浏览器，搜索阿里云

# 2、点击产品，搜索镜像，出现容器镜像服务

# 3、点击控制台，在镜像工具中点击镜像加速器

# 4、获取镜像加速地址，将命令执行
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://ahcy678m.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```



docker服务相关命令

```shell
# 启动服务
systemctl start docker

# 停止服务
systemctl stop docker

# 重启服务
systemctl restart docker

# 设置开机启动
systemctl enable docker
```



docker镜像相关命令

查看更多命令可以在浏览器中搜索“docker命令大全”

```shell
# 查看镜像
docker images

# 搜索镜像
docker search redis

# 拉取镜像
docker pull redis
docker pull redis:5.0

# 删除镜像（对应的ID）
docker rmi 7614ae9453d1
docker rmi redis:latest

# 一次性删除所有镜像
docker images -q    # 查看所有镜像的ID
docker rmi `docker images -q` # 这里不是单引号
```

查找docker中的镜像版本，dockerhub



docker容器相关的命令

```shell
# 查看容器
docker ps -a
-a 查看历史容器（包含当前容器和已关闭容器）

# 创建容器
docker run -i -t redis:latest /bin/bash
-i 设置容器一直运行
-t 开启终端接收命令
/bin/bash 指定终端的版本
exit 退出容器后自动关闭

docker run -i -d redis:latest
-d 容器后台运行
exit 使用此方法创建的容器使用exit也不会关闭容器

# 进入容器
docker exec -i -t c1 /bin/bash
c1 容器的名字

# 启动容器

# 停止容器
docker stop c1
c1 容器的名字

# 删除容器  
docker rm c1 # 也可以使用ID删除
c1 容器的名字

# 查看所有容器的ID
docker ps -aq
docker rm `docker ps -aq` # 删除所有容器
不能删除正在运行的容器

# 查看容器信息
docker inspect c1
c1 容器的名字
```



docker容器的数据卷

