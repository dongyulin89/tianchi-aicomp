# 安装
## Linux
```shell
$ sudo curl -sS https://get.docker.com/ | sh
```
测试
```shell
$ docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
d1725b59e92d: Pull complete
Digest: sha256:0add3ace90ecb4adbf7777e9aacf18357296e799f81cabc9fde470971e499788
Status: Downloaded newer image for hello-world:latest
Hello from Docker!
This message shows that your installation appears to be working correctly.
To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.
To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash
Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/
For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```
> 出错：docker: Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post http://%2Fvar%2Frun%2Fdocker.sock/v1.40/containers/create: dial unix /var/run/docker.sock: connect: permission denied.
See 'docker run --help'.  
> 解决方案：
> ```shell
> sudo groupadd docker
> sudo gpasswd -a $ldy docker
> sudo gpasswd -a $USER docker
> newgrp docker
> ```
> 参考资料：[link](https://blog.csdn.net/liangllhahaha/article/details/92077065)

如果机器有支持深度学习的GPU，可以继续执行如下命令以支持容器对gpu的调用[目前仅支持linux]：
```shell
# Add the package repositories
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list


sudo apt-get update && sudo apt-get install -y nvidia-container-toolkit
sudo systemctl restart docker
```

# docker 基础命令学习

## 拉取镜像
```shell
docker pull [选项] [docker镜像地址:标签]
```

如：
```shell
docker pull hello-word:latest
```

## 运行镜像
```shell
$ docker run hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

## 运行镜像并进入容器
```shell
$ docker run -it --rm ubuntu:18.04 bash
root@e7009c6ce357:/#  uname -a
Linux bff9f261bab2 4.15.0-106-generic #107-Ubuntu SMP Thu Jun 4 11:27:52 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
root@e7009c6ce357:/# exit
```
`docker run`就是运行容器的命令，后面如果只跟镜像，那么就执行镜像的默认命令然后退出
- `it`： 这是两个参数，一个是`-i`：交互操作，一个是`-t`终端。我们这里打算进入`bash`执行一些命令并查看返回结果，因此我们需要交互式终端。
- `--rm`：这个参数是说容器退出之后将其删除。默认情况下，为了排障需求，退出的容器并不会立即删除，除非手动 `docker rm`。我们这里只是随便执行个命令，看看结果，不需要排障和保留结果，因此使用 `--rm` 可以避免浪费空间。
- `ubuntu:18.04`：这是指用 `ubuntu:18.04` 镜像为基础来启动容器。
- `bash`：放在镜像名后的是 **命令** ，这里我们希望有个交互式 `Shell`，因此用的是 `bash`。

进入容器后，我们可以在 `Shell` 下操作，执行任何所需的命令。通过 `exit` 退出。

## 查看本地镜像（list 镜像）
```shell
$ docker images
REPOSITORY           TAG                 IMAGE ID            CREATED             SIZE
redis                latest              5f515359c7f8        5 days ago          183 MB
nginx                latest              05a60462f8ba        5 days ago          181 MB
```
`IMAGE ID` 是镜像的唯一标识。

## 查看运行中的容器
```shell
$ docker ps
CONTAINER ID        IMAGE          COMMAND      CREATED             STATUS              PORTS               NAMES
9363b1b51118        testlog:1      "bash"       7 weeks ago         Up 7 weeks                              vigilant_bhaskara
```
`CONTAINER ID` 容器唯一id 可以通过指定这个ID操作 `exec shell` 进入容器、`commit` 这个容器的修改、`tag` 给这个容器打标签等，`docker ps` 罗列的是当前活跃的容器，要查看所有容器执行`docker ps -a`。

## 进入运行中/后台运行的容器
```shell
$ docker exec -it [CONTAINER ID] /bin/bash
```
进入运行中的容器后不仅可以调试镜像，还可以对镜像做修改，如安装python包，如果想对修改做保留，可以执行 `commit`。

## 保存修改
```shell
docker commit [CONTAINER ID] registry.cn-shanghai.aliyuncs.com/test/pytorch:myversion
```
注意：通过commint的形式保存现场为一个新的镜像虽然也能直观的达到构建新镜像的目的，但是实际操作中，并不推荐这种形式，因为：
1. commit操作不仅会把有用的修改保存下来，对一些无关的修改也会保存下来（每一个命令行操作都会生成存储如ls操作）就会导致镜像比较臃肿；
2. commit操作属于黑箱操作，后续如果有什么问题维护起来会比较麻烦。 

建议commit仅作为保留现场的手段，然后通过修改dockerfile构建镜像。

## 打 TAG
```shell
docker tag registry.cn-shanghai.aliyuncs.com/test/pytorch:myversion my_tmp_version:0.1
```
有时需要对临时版本，或者节点版本做一个标记保留，打TAG标签非常好用，并不会额外占用空间。

## 推送镜像到仓库
```shell
docker push registry.cn-shanghai.aliyuncs.com/test/pytorch:myversion
```
