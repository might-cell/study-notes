开发环境：编写代码

测试环境：项目部署，编写测试单元

生产环境：正式运行



如果运行时环境的版本没有进行事先约定，代码在不同环境上运行可能会出现“水土不服”的症状



docker：将代码和环境（开发时程序可以正常运行的环境）放在一个容器中，测试和运行直接在容器中进行，确保代码不会出现“水土不服”的情况

* 开源应用容器引擎
* 基于Go语言实现
* 开发者打包应用以及依赖包到一个轻量级、可移植的容器中，这个docker容器发布到任何流行版本的Linux上
* 容器使用沙箱机制，相互隔离，可以同时启动多个容器，并且相互之间不会产生影响
* 性能开销低，运行速度极快



**docker是一种容器技术，主要解决软件跨环境迁移的问题**



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
* 容器（Container）：镜像和容器的关系，就像是面向对象程序设计中的类和对象一样，镜像是静态的定义，容器是镜像运行的实体。容器可以被创建、启动、停止、删除、暂停等
* 仓库（Repository）：仓库可以看做一个代码控制中心，用来统一保存镜像



配置docker镜像加速器

默认情况下，从dockerhub上下载docker镜像，速度很慢，一般会配置镜像加速器



下面使用阿里云提供的镜像加速器



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

# 拉取镜像（如果不指定版本，docker默认拉取latest版本，如果想要知道docker支持了哪些版本，需要在dockerhub中查看）
docker pull redis
docker pull redis:5.0

# 删除镜像（对应的ID或者名字:版本）
docker rmi 7614ae9453d1 # 这里的ID需要查看镜像信息获取
docker rmi redis:latest

# 一次性删除所有镜像
docker images -q    # 查看所有镜像的ID
docker rmi `docker images -q` # 这里不是单引号，这里的符号是Tab键上方的按键
```

查找docker中的镜像版本，dockerhub



docker容器相关的命令

```shell
# 查看容器（如果不指定-a参数，那么只能查看当前运行中的容器）
docker ps -a
-a 查看历史容器（包含当前容器和已关闭容器）

# 创建容器
docker run -i -t redis:latest /bin/bash
-i 设置容器一直运行
-t 开启一个终端窗口来接收后续命令
/bin/bash 指定开启的终端的版本
exit 退出容器后自动关闭容器

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

docker容器删除后，在容器中产生的数据也不存在了

docker容器无法直接和外部机器交换文件

多个docker容器之间无法直接进行数据交互



数据卷

* 数据卷是<span style="color:yellow">宿主机</span>中的一个目录或者文件

将数据卷挂载在容器上，在容器中发生了变化，那么数据卷中也会发生对应变化

* 当容器目录和数据卷绑定后，对方的修改会立即同步
* 一个数据卷可以被多个容器同时挂载
* 一个容器也可以挂载多个数据卷



因此数据卷的作用：

* 容器数据的持久化存储，不会因为容器的删除而丢失容器中的数据
* 外部机器和容器之间可以借助数据卷来实现间接通信
* 容器之间也是借助同时挂载一个相同的数据卷来进行容器之间的数据交换



配置数据卷

```shell
# 创建启动容器的时候，使用-v参数来设置数据均
docker run 省略 -v
-v 宿主机的目录/文件：容器内目录/文件 省略

docker run -it --name=c1 -v /root/data:/root/data_container redis:latest /bin/bash
/root/data 宿主机上的一个文件或者目录，简称为数据卷
/root/data_container docker容器内的一个目录，与宿主机上的数据卷进行映射
resis:latest 指定镜像
```

1、目录必须是绝对路径

2、如果目录不存在，就会自动创建

3、一个容器可以挂载多个数据卷