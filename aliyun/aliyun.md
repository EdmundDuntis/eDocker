

### 阿里云Docker实操

#### 创建阿里云实例

ubuntu18.04

#### 创建用户

使用root操作

```bash
#创建用户
adduser work
#按照提示设置密码并输入相关信息

#将work加入sudoers file中
visudo
#进入编辑
root    ALL=(ALL:ALL) ALL
#后加一行，Ctrl+x按提示保存
work    ALL=(ALL:ALL) ALL

#验证一下
su - work
sudo apt-get install curl
```

#### 安装docker

使用work操作

```bash
#删除之前版本
sudo apt-get remove docker \
               docker-engine \
               docker.io
sudo apt-get update

#安装相关软件
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common

#获取证书
curl -fsSL https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu/gpg | sudo apt-key add -

#添加安装源
sudo add-apt-repository \
    "deb [arch=amd64] https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu \
    $(lsb_release -cs) \
    stable"
sudo apt-get update

#安装
sudo apt-get install docker-ce

#启动docker
sudo systemctl enable docker
sudo systemctl start docker

#添加用户组
sudo groupadd docker
sudo usermod -aG docker $USER
```

#### 设置阿里云镜像加速

```bash
#阿里云控制台到转到"容器镜像服务"
#在镜像加速器 获得 加速地址，并按操作步骤操作配置镜像加速器
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://****.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker

#验证，redis镜像接近100M很快获取到
docker pull redis
```

#### 构建镜像

```bash
#创建Dockerfile
FROM nginx
RUN echo '<h1>Hello, Docker file!</h1><h1>Edmund Duntis</h1>' > /usr/share/nginx/html/index.html

#构建镜像
docker build -t enginx .
#运行容器测试一下
docker run -d -p 80:80  enginx
curl 127.0.0.1

#阿里云控制台转到容器镜像服务，创建仓库
#进入创建的参考，按照提示操作
sudo docker login --username=****@gmail.com registry-vpc.cn-huhehaote.aliyuncs.com
sudo docker tag [ImageId] registry-vpc.cn-huhehaote.aliyuncs.com/edmund/duntis:1.0.1
sudo docker push registry-vpc.cn-huhehaote.aliyuncs.com/edmund/duntis:1.0.1
#在阿里云容器镜像服务的镜像版本可以看到push的镜像文件

#删除本地镜像后从阿里云私有仓库拉取
sudo docker pull registry-vpc.cn-huhehaote.aliyuncs.com/edmund/duntis:1.0.1

#验证
docker run -d -p 81:80  registry-vpc.cn-huhehaote.aliyuncs.com/edmund/duntis:1.0.1
curl 127.0.0.1:81
```

