

安装docker

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

