# FISCO-BCOS_Build_Tutorial

How to Construct [FISCO-BCOS](https://github.com/FISCO-BCOS/FISCO-BCOS) Consortium Chain and Common Errors

This is [shadowNo-1](https://github.com/shadowNo-1)'s private note

[![Author](https://img.shields.io/badge/author-shadowNo--1-informational?style=flat&logo=github&logoColor=181717&color=green)](https://github.com/shadowNo-1)
![](https://img.shields.io/badge/license-GNU-informational?style=flat&logo=gnu&logoColor=white&color=A42E2B)
![](https://img.shields.io/badge/Ubuntu-22.04.3-informational?style=flat&logo=ubuntu&logoColor=e95420&color=e95420)
[![FISCO--BCOS](https://is.gd/V01vho)](https://fisco-bcos-documentation.readthedocs.io/zh_CN/latest/)

## FISCO-BCOS 版本说明
FISCO-BCOS目前主要存在[2.x](https://fisco-bcos-documentation.readthedocs.io/zh_CN/latest/)和[3.x](https://fisco-bcos-doc.readthedocs.io/zh-cn/latest/)两个大版本，用户可以根据不同的场景选择:

- [v2.x](https://fisco-bcos-documentation.readthedocs.io/zh_CN/latest/)与[v3.x](https://fisco-bcos-doc.readthedocs.io/zh-cn/latest/)版本**互不兼容**，但后续会同时保持维护、迭代、更新。

- [v2.x](https://fisco-bcos-documentation.readthedocs.io/zh_CN/latest/)是目前的稳定版本，已经经过多个机构、多个应用，长时间在生产环境中的实践检验，具备金融级的高性能、高可用性及高安全性。该版本会持续进行维护。用户当前有生产级的使用需求，可以直接使用[v2.x](https://fisco-bcos-documentation.readthedocs.io/zh_CN/latest/)即可。

- [v3.x](https://fisco-bcos-doc.readthedocs.io/zh-cn/latest/)为当前FISCO-BCOS最新的版本，采用微服务的架构，已发布正式版。

## 1、基础环境配置
> [!IMPORTANT]
> **本文以`Ubuntu 22.04.3`为案例**
- ### 备份源列表

Ubuntu采用`apt`作为软件安装工具，其镜像源列表记录在`/etc/apt/source.list`文件中。
将`source.list`复制为`source.list.bak`备份

```
sudo cp /etc/apt/sources.list /etc/apt/sources.list_bak
```

- ### 镜像源列表替换
使用[gedit](https://github.com/GNOME/gedit)或[vim](https://github.com/vim/vim)修改`sources.list`文件
```
gedit /etc/apt/sources.list
```
建议将常用镜像源保存在`/etc/apt`目录下，并命名为类似`source.list.源名称`的形式，需要使用时直接复制替换`source.list`文件即可。

### 可用的中国内地开源镜像站
> [!TIP]
> 如需用于其他版本，将`jammy`换成其他版本代号即可: ***22.04***:`jammy`；***20.04***:`focal`；***18.04***:`bionic`；***16.04***:`xenial`；***14.04***:`trusty`。
- #### 清华源<sub>（截止至2023-11-28T16:50:06 GMT+8）</sub>
请选择对应版本的源替换，详见清华大学开源软件镜像站[help](https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/)
```bash
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse

deb http://security.ubuntu.com/ubuntu/ jammy-security main restricted universe multiverse
# deb-src http://security.ubuntu.com/ubuntu/ jammy-security main restricted universe multiverse

# 预发布软件源，不建议启用
# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-proposed main restricted universe multiverse
# # deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-proposed main restricted universe multiverse
```
- #### 中科大源<sub>（截止至2023-11-28T17:15:15 GMT+8）</sub>
请选择对应版本的源替换，详见中国科学技术大学开源软件镜像站[help](https://mirrors.ustc.edu.cn/help/ubuntu.html)

中国科学技术大学开源软件镜像站[存储库文件生成器](https://mirrors.ustc.edu.cn/repogen/)
```bash
# 默认注释了源码仓库，如有需要可自行取消注释
deb https://mirrors.ustc.edu.cn/ubuntu/ jammy main restricted universe multiverse
# deb-src https://mirrors.ustc.edu.cn/ubuntu/ jammy main restricted universe multiverse

deb https://mirrors.ustc.edu.cn/ubuntu/ jammy-security main restricted universe multiverse
# deb-src https://mirrors.ustc.edu.cn/ubuntu/ jammy-security main restricted universe multiverse

deb https://mirrors.ustc.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
# deb-src https://mirrors.ustc.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse

deb https://mirrors.ustc.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse
# deb-src https://mirrors.ustc.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse

# 预发布软件源，不建议启用
# deb https://mirrors.ustc.edu.cn/ubuntu/ jammy-proposed main restricted universe multiverse
# deb-src https://mirrors.ustc.edu.cn/ubuntu/ jammy-proposed main restricted universe multiverse
```
- #### 浙大源<sub>（截止至2023-11-29T09:24:05 GMT+8）</sub>
请选择对应版本的源替换，详见浙江大学开源软件镜像站[docs](http://mirrors.zju.edu.cn/docs/ubuntu/)
```bash
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.zju.edu.cn/ubuntu/ jammy main restricted universe multiverse
# deb-src https://mirrors.zju.edu.cn/ubuntu/ jammy main restricted universe multiverse
deb https://mirrors.zju.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
# deb-src https://mirrors.zju.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
deb https://mirrors.zju.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse
# deb-src https://mirrors.zju.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse
deb https://mirrors.zju.edu.cn/ubuntu/ jammy-security main restricted universe multiverse
# deb-src https://mirrors.zju.edu.cn/ubuntu/ jammy-security main restricted universe multiverse

# 预发布软件源，不建议启用
# deb https://mirrors.zju.edu.cn/ubuntu/ jammy-proposed main restricted universe multiverse
# deb-src https://mirrors.zju.edu.cn/ubuntu/ jammy-proposed main restricted universe multiverse
```

## 2、搭建第一个区块链网络


## Golang

1、安装curl、git

apt-get update
 
apt-get install git
 
apt-get install curl



2、安装go
curl -O https://storage.googleapis.com/golang/go1.13.4.linux-amd64.tar.gz

tar -C /usr/local -xzf go1.13.4.linux-amd64.tar.gz



3、配置go的环境变量
mkdir -p ~/go; echo "export GOPATH=$HOME/go" >> ~/.bashrc
 
echo "export PATH=$PATH:$HOME/go/bin:/usr/local/go/bin" >> ~/.bashrc
 
source ~/.bashrc

验证go :
go version


4、安装以太坊环境
sudo apt-get install software-properties-common
 
sudo add-apt-repository -y ppa:ethereum/ethereum
 
sudo add-apt-repository -y ppa:ethereum/ethereum-dev
 
sudo apt-get update
 
sudo apt-get install ethereum



5、安装solc（solidity编译环境）

sudo add-apt-repository ppa:ethereum/ethereum
 
sudo apt-get update
 
sudo apt-get install solc
