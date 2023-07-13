# Docker

* [Docker | AcWing Linux 基础课](https://www.acwing.com/blog/content/10878/)
* [Docker 入门教程 | 阮一峰的网络日志](https://www.ruanyifeng.com/blog/2018/02/docker-tutorial.html)

***

## Docker是什么？

> **Docker 是一个开源的应用容器引擎，让开发者可以打包他们的应用以及依赖包到一个可移植的容器中,然后发布到任何流行的Linux或Windows操作系统的机器上**

## Docker常用命令

* `镜像（images）`
  1. `docker images`：列出本地所有镜像
  2. `docker image rm ubuntu:20.04`：删除镜像`ubuntu:20.04`
  3. `docker commit CONTAINER_NAME IMAGE_NAME:TAG`：创建某个`container`的镜像，`TAG` 为镜像标签，用以记录当前版本。
  4. `docker save -o ubuntu_20_04.tar ubuntu:20.04`：将镜像`ubuntu:20.04`导出到本地文件`ubuntu_20_04.tar`中
  5. `docker load -i ubuntu_20_04.tar`：将镜像`ubuntu:20.04`从本地文件`ubuntu_20_04.tar`中加载出来
* `容器(container)`
  1. `docker ps -a`：查看本地的所有容器
  2. `docker start CONTAINER`：启动容器
  3. `docker stop CONTAINER`：停止容器
  4. `docker restart CONTAINER`：重启容器
  5. `docker rm CONTAINER`：删除容器
  6. `docker run -p HOST_PORT:CONTAINER_PORT --name CONTAINER_NAME -itd IMAGE_NAME:TAG`：将创建并启动一个容器
     * `-p`：端口映射，将宿主机的端口和容器的端口进行映射
       * 例：`-p 20000:22 -p 8000:8000 -p 80:80 -p 443:443`
       * `22`：`ssh`登录服务端口
       * `8000`：`Django`调试端口
       * `80`：用于`HTTP`服务
       * `443`：用于`HTTPS`服务
  7. `docker attach CONTAINER`：进入容器
     * 先按`Ctrl-p`，再按`Ctrl-q`可以挂起容器

## Docker环境配置

1. `scp django_lesson_1_0.tar server`：将`docker压缩包`传至云服务器
2. `ssh server`：免密登录至云服务器
3. `docker load -i django_lesson_1_0.tar`：将`docker压缩包`解压缩成`docker镜像`
4. `docker run -p 20000:22 8000:8000 --name django -itd django_lesson:1.0` ：利用 `镜像django_lesson:1.0` 创建一个命名为 `django` 的 `docker容器`并启动
5. `docker attach my_docker_server`：进入创建的`docker容器`（服务器）
6. `adduser acs`：创建`acs`用户
7. `usermod -aG sudo acs`：为`acs`用户分配`sudo`权限
8. `scp .bashrc .vimrc .tmux.conf django:`：将本地服务器的`bash`&`vim`&`tmux`配置文件传至`docker 容器`

## Docker项目迁移

第一步，登录容器，关闭所有运行中的任务

第二步，登录运行容器的服务器，然后执行：

```
docker commit CONTAINER_NAME django_lesson:1.1  # 将容器保存成镜像，将CONTAINER_NAME替换成容器名称
docker stop CONTAINER_NAME # 关闭容器
docker rm CONTAINER_NAME # 删除容器
```

#### 增加容器的映射端口 : 80 与 443

> **给运行中的容器，开通端口，是一件非常麻烦的事情**
>
> **一个比较好的解决方案 : 先把容器保存成镜像，再删掉容器，然后用镜像生成新的容器，同时打开需要的端口**

第一步，登录容器，关闭所有运行中的任务

第二步，登录运行容器的服务器，然后执行 :

```shell
docker commit CONTAINER_NAME django_lesson:1.1  # 将容器保存成镜像，将CONTAINER_NAME替换成容器名称
docker stop CONTAINER_NAME  # 关闭容器
docker rm CONTAINER_NAME # 删除容器

# 使用保存的镜像重新创建容器

docker run -p 20000:22 -p 8000:8000 -p 80:80 -p 443:443 --name CONTAINER_NAME -itd django_lesson:1.1
```

第三步，去云服务器控制台，在安全组配置中开放80和443端口
