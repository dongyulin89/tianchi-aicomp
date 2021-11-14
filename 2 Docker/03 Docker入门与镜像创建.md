这两个视频可以看一下：[课时6](https://tianchi.aliyun.com/course/351/4129?spm=5176.20850343.J_3678908510.7.572b2854AVmJk3) [课时7](https://tianchi.aliyun.com/course/351/4131?spm=5176.20850343.J_3678908510.7.572b2854AVmJk3)

# 使用 dockerfile 构建镜像
Dockerfile示例(注意一般文件名命名为Dockerfile 无后缀名，如果命名为其他名字，构建时需要额外指定文件名)
```shell
# Base Images
## 从天池基础镜像构建(from的base img 根据自己的需要更换，建议使用天池open list镜像链接：https://tianchi.aliyun.com/forum/postDetail?postId=67720)

# 安装 Python3 基础镜像
FROM registry.cn-shanghai.aliyuncs.com/tcc-public/python:3

## 安装 依赖包（-i 指定源）
RUN pip install numpy -i https://pypi.tuna.tsinghua.edu.cn/simple 
##或者从requirements.txt安装（不推荐）
##RUN pip install --no-cache-dir -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple 

## 把当前文件夹里的文件构建到镜像的根目录下（代码之类的）
ADD . /workfile 
## 并设置为默认工作目录
WORKDIR /workfile

## 镜像启动后统一执行 sh run.sh
CMD ["sh", "run.sh"]
```

## Linux 上面创建 Dockerfile 文件
```shell
vim Dockerfile 
```
然后把上一步的代码粘贴到 Dockerfile 文件中并保存。

## 构建镜像
```shell
docker build -t registry.cn-shanghai.aliyuncs.com/target:test .
```
默认使用 Dockerfile 这个名字，docker 会自动找到该文件，并进行构建镜像。

如要指定docker：
```shell
docker build -f ./dockerfile -t registry.cn-shanghai.aliyuncs.com/target:test .
```
如果 Dockerfile 的名字改成其他的，比如“dockerdd”，就需要添加 `-f ./dockerdd` 寻找该 docker 文件进行构建。

## 删除容器/镜像
删除镜像：
```shell
docker rmi registry.cn-shanghai.aliyuncs.com/target:test
```

删除容器：
```shell
docker rm [CONTAINER ID]
```

如果容器还在运行，则会删除失败，应该先结束掉容器：
```shell
docker kill [CONTAINER ID]
```

查看运行中的容器：
```shell
docker ps
```

查看所有容器：
```shell
docker ps -a 
```

## 常规技巧
1. 检查基础镜像软件源和pip源是否替换为国内源，如果非国内源后续每次构建镜像会比较浪费时间。 
2. 必备软件包可直接安装于基础镜像内，以减少每次构建镜像时都要安装一遍的等待时间。 
3. 镜像面临调试问题时，可交互式进入容器后直接调试修改，直到成功后退出再在dockerfile中修改。 
4. 养成使用Dockerfile的习惯，不要依赖于commit。
5. 每次镜像修改都给定新的版本号或标签，方便区分版本管理，有意义的版本最好使用有含义的字符作为版本号，如：frist_submit

## 深度学习常用镜像集合（包含国内源和海外源）
[https://tianchi.aliyun.com/forum/postDetail?postId=67720](https://tianchi.aliyun.com/forum/postDetail?postId=67720)

# 如何创建自己的基础镜像
避免在每次构建镜像的时候，都需要添加相同的软件包，这时候可以构建自己的基础镜像减少麻烦。之后 ADD 我们自己的代码，再构建镜像就很方便啦。

##
从[阿里云的官方镜像](https://tianchi.aliyun.com/forum/postDetail?postId=67720)中添加自己要用的镜像。  
比方说这个：  
![image](https://user-images.githubusercontent.com/44680953/141688311-fe46b5c4-ba16-4c34-83dc-36bebba80e87.png)

```shell
docker pull  registry.cn-shanghai.aliyuncs.com/tcc-public/pytorch:latest-py3 
```

后台运行 docker 镜像
```shell
docker run -itd  registry.cn-shanghai.aliyuncs.com/tcc-public/pytorch:latest-py3 
```
`-d`：后台运行

如何查看后台运行的镜像？
```shell
docker ps
```

进入该镜像并修改镜像
```shell
docker exec -it [IMAGE ID] /bin/bash
```

安装需要添加的依赖包
```shell
pip install numpy
pip install pandas
```

退出镜像
```shell
exit
```

保存修改
```shell
docker commit [IMAGE ID] registry.cn-shanghai.aliyuncs.com/tcc-public/pytorch:latest-py3: myversion
```
之后再 FROM 这个 pytorch 镜像的时候就会包含 numpy 和 pandas 的包了！

这时候查看本地的镜像
```shell
docker images
```
已经包含我们修改过的镜像了  
![image](https://user-images.githubusercontent.com/44680953/141688700-2a708275-ee64-4d8e-9989-ff1a49e709ed.png)  

运行我们的镜像验证一下（myversion 不要忘记添加）
```shell
docker run -it registry.cn-shanghai.aliyuncs.com/tcc-public/pytorch: myversion /bin/bash
```

查看是否已经添加 pandas 包
```shell
$ pip list|grep pandas

pandas 1.1.5
```
