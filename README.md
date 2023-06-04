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

## 数据卷是什么

1. 数据卷就是宿主机上的一个目录或者一个文件（只是这样的话，还不能称之为数据卷，需要将目录与容器进行一个挂载起来，这样宿主机的目录就被称为数据卷，
2. 这样之后宿主机的目录和容器的目录绑定之后，双方的修改会立即同步。
3. 一个数据卷可以被多个容器挂载

### 配置数据卷

1. 在启动容器时，使用-v 参数设置数据卷

```
docker run ... -v 宿主机目录(文件):容器内目录(文件)... centos:7 /bin/bash
```

2. 注意事项
   - 目录必须是绝对路径
   - 如果目录不存在，会自动创建
   - 可以挂载多个数据卷

#### 配置数据卷容器

```
核心就是先创建一个容器c3使其与一个数据卷关联，随后在将c1，c2与容器c3进行关联，这样也就形成了容器c1，c2与数据卷的关联。
```

1. 创建启动 c3 数据卷容器，使用-v 参数 设置数据卷

```
docker run -it --name=c3 -v /volume centos:7 /bin/bash   （/volume是容器目录，使用这种方式宿主机会自动分配一个目录给我们当作数据卷
```

2. c1 与 c3 进行绑定（c2 与此相同

```
docker run -it --name=c1 --volume-from c3 centos:7
```

## Dockerfile

**此文件是用来制作 docker 镜像的**

### Docker 镜像原理

1. Docker 镜像是由特殊的文件系统叠加而成
2. 最低端是 bootfs，并使用宿主机的 bootfs
3. 第二层是 root 文件系统 rootfs，称之为 base image（基础镜像
4. 在往上可以叠加其他的镜像文件
5. 这种叠加的操作称之为统一文件系统(Union File System)技术能够将不同的层整合为一个文件系统。为这些层提供了一个统一的视角，这样就隐藏了多层的存在，在用户的角度看来，只存在一个文件系统。
6. 一个镜像可以放在另一个镜像的上面。位于下面的镜像称之为父镜像，最低部的称为基础镜像。
7. 当从一个镜像启动容器时，Docker 会在最顶层加载一个读写文件系统作为容器

### Docker 镜像制作
