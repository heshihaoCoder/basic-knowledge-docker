## docker 服务相关的命令

- systemctl start docker : 启动 docker (centos7)
- systemctl status docker : 查看 docker 状态
- systemctl stop docker : 暂停 docker 服务
- systemctl restart docker : 重启 docker 服务
- systemctl enable docker : 开机启动 docker 服务

## docker 镜像相关的命令

- docker images : 查看本地有哪些镜像
- docker search （应用名称） : 搜索镜像
- docker pull (应用名称)指定版本的话就是在后面加上:版本号,不写版本号就是 latest 版本 : 下载镜像
- docker rmi 对应的镜像 id : 删除镜像 也可以通过 **docker rmi 镜像名字加版本号**进行删除镜像
- docker rmi \`docker images -q` : 删除所有的镜像文件 (**docker images -q 查看所有镜像的 id**

## docker 容器相关的命令

- docker run -i -t --name=c1 centos:7 /bin/bash : 创建容器，然后会自动进入容器。参数含义:(没有客户端连接会自动关闭加上-i 表示一直运行);(-t 表示给容器分配一个终端来输入命令);(--name=c1 表示容器的名字就是 c1);(centos:7 表示根据哪个镜像创建容器:后面指定版本);(/bin/bash 表示进入容器的初始化指令)

- exit : 退出容器

- docker ps : 查看正在运行的容器（加上-a 的话就是看所有的容器

- docker run -i -d --name=c1 centos:7 /bin/bash : 创建容器，会在后台运行需要自己进入容器。参数含义:(没有客户端连接会自动关闭加上-i 表示一直运行);(-d 表示不会自动进入容器);(--name=c1 表示容器的名字就是 c1);(centos:7 表示根据哪个镜像创建容器:后面指定版本);(/bin/bash 表示进入容器的初始化指令) 命令输入之后会返回一个容器 id, 这样创建的容器在 `docker exit`退出之后，容器不会关闭

- docker exec -it c2 : 进入容器 (c2 : 表示进入的容器的名字)

- docker stop 容器名称 : 停止容器
- docker start 容器名称 : 启动容器
- docker rm 容器名称（容器 id 也可以） : 删除容器
- docker rm \`docker ps -aq` : 删除所有的容器（开启的容器是不能被删除的
- docker inspect 容器名称（容器 id 也可以） : 查看容器信息
