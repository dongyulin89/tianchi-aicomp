这里以申请阿里云容器镜像服务（免费），并创建仓库为例，其他仓库如dockerhub、谷歌、亚马逊、腾讯等详见对应产品说明书。 打开阿里云容器服务地址为（[https://cr.console.aliyun.com](https://cr.console.aliyun.com)） 注册开通后产品页面如下：  
![image](https://user-images.githubusercontent.com/44680953/141689363-93115d71-81dd-4d9c-b207-3bdc96ba66d5.png)  

第一步切换标签页到命名空间，创建地址唯一的命名空间  
![image](https://user-images.githubusercontent.com/44680953/141689378-254b68a1-1f13-4c4c-85da-71eeb235f508.png)  


第二步创建镜像仓库，据大赛要求选择对应的地域，其他的按照自己需求选择或填写
![image](https://user-images.githubusercontent.com/44680953/141689405-a0c5e19e-0ccf-4742-879a-0499c162367a.png)  

下一步，选择本地仓库，不建议其他选项，完成创建  
![image](https://user-images.githubusercontent.com/44680953/141689413-a0e0e65c-4c3a-440c-88bc-810b3313e476.png)  

点击管理，可查看详情  
![image](https://user-images.githubusercontent.com/44680953/141689426-9bcb87ba-4bb6-4bbd-a97a-bd4d004945ec.png)  

详情页如下，有基本的操作命令，仓库地址一般使用公网地址即可  
![image](https://user-images.githubusercontent.com/44680953/141689437-92e9cd7c-2d4d-4e8f-9a39-923474049919.png)  

按照页面的指令在本地完成登陆:
```shell
$ export DOCKER_REGISTRY=your_registry_url
$ docker login $DOCKER_REGISTRY \
> --username your_username 
Password: 

Login Succeeded
```

# GPU 版 docker 练习赛实践
比赛链接：[零基础入门Docker-cuda练习场【免费GPU】](https://tianchi.aliyun.com/competition/entrance/531863/information?spm=5176.21852674.0.0.24b96bdfWod2NL)

首先我们写一个`math.py`，实现用一个矩阵创建cuda张量，在GPU上进行矩阵乘法：
```shell
vim math.py
```

```python
# math.py
import torch
device = torch.device("cuda")
a = torch.randn(3, 3)
b = torch.randn(3, 3)
a = a.to(device)
b = b.to(device)
c = torch.matmul(a,b)
print(c)
```

编写入口文件run.sh
```shell
vim run.sh
```

```shell
#bin/bash
#打印GPU信息
nvidia-smi
#执行math.py
python3 math.py
```

然后编写 Dockerfile 用于打包main.py和运行环境为镜像
```shell
vim Dockerfile
```

```shell
# Base Images
## 从天池基础镜像构建(from的base img 根据自己的需要更换，建议使用天池open list镜像链接：https://tianchi.aliyun.com/forum/postDetail?postId=67720)
FROM registry.cn-shanghai.aliyuncs.com/tcc-public/pytorch:1.4-cuda10.1-py3

##安装依赖包,pip包请在requirements.txt添加
#RUN pip install --no-cache-dir -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple

## 把当前文件夹里的文件构建到镜像的//workspace目录下,并设置为默认工作目录
ADD math.py /workspace
ADD run.sh /workspace
WORKDIR /workspace

## 镜像启动后统一执行 sh run.sh
CMD ["sh", "run.sh"]
```

命令行执行，构建镜像：   
tips: 镜像命名根据自己申请的仓库 registry 来，可以省去tag步骤直接上传，保持本地镜像清洁
```shell
$ docker build -t registry.cn-hangzhou.aliyuncs.com/ldy_test_for_tianchi/ldy_test_for_tianchi_submit:0.1 .
```

上传镜像仓库
```shell
$ docker push registry.cn-hangzhou.aliyuncs.com/ldy_test_for_tianchi/ldy_test_for_tianchi_submit:0.1
```

天池页面提交     
![image](https://user-images.githubusercontent.com/44680953/141695671-83fb89e1-8074-4182-92ac-f86b4c0585e5.png)
