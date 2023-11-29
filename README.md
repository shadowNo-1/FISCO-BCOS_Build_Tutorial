# FISCO-BCOS_Build_Tutorial

How to Construct [FISCO-BCOS](https://github.com/FISCO-BCOS/FISCO-BCOS) Consortium Chain and Common Errors

This is [shadowNo-1](https://github.com/shadowNo-1)'s private note

[![Author](https://img.shields.io/badge/author-shadowNo--1-informational?style=flat&logo=github&logoColor=181717&color=green)](https://github.com/shadowNo-1)
![](https://img.shields.io/badge/license-GNU-informational?style=flat&logo=gnu&logoColor=white&color=A42E2B)
![](https://img.shields.io/badge/Ubuntu-22.04.3-informational?style=flat&logo=ubuntu&logoColor=e95420&color=e95420)
[![FISCO--BCOS](https://is.gd/71u78p)](https://fisco-bcos-documentation.readthedocs.io/zh_CN/latest/)

## FISCO-BCOS 版本说明<sub>*(截止至2023-11-28 T 13:46:11 GMT+8)*</sub>
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
  
- ### 镜像源列表替换<sub>[可用的中国内地开源镜像站](https://github.com/shadowNo-1/FISCO-BCOS_Build_Tutorial/#%E5%8F%AF%E7%94%A8%E7%9A%84%E4%B8%AD%E5%9B%BD%E5%86%85%E5%9C%B0%E5%BC%80%E6%BA%90%E9%95%9C%E5%83%8F%E7%AB%99)</sub>
  使用[gedit](https://github.com/GNOME/gedit)或[vim](https://github.com/vim/vim)修改`sources.list`文件
  ```
  gedit /etc/apt/sources.list
  ```
  建议将常用镜像源保存在`/etc/apt`目录下，并命名为类似`source.list.源名称`的形式，需要使用时直接复制替换`source.list`文件即可。

- ### 镜像源列表更新
  修改完成后保存`source.list`文件后执行更新
  ```bash
  sudo apt update
  ```

### 可用的中国内地开源镜像站
> [!TIP]
> 如需用于其他版本，将`jammy`换成其他版本代号即可: ***22.04***:`jammy`；***20.04***:`focal`；***18.04***:`bionic`；***16.04***:`xenial`；***14.04***:`trusty`。

- #### 清华源<sub>（截止至2023-11-28 T 16:50:06 GMT+8）</sub>
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
  
- #### 中科大源<sub>（截止至2023-11-28 T 17:15:15 GMT+8）</sub>
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
  
- #### 浙大源<sub>（截止至2023-11-29 T 09:24:05 GMT+8）</sub>
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


# 搭建第一个区块链网络
## 1. 搭建单群组FISCO BCOS联盟链
- ### 第一步. 安装依赖
  安装ubuntu依赖
  ```bash
  sudo apt install -y openssl curl
  ```
- ### 第二步. 创建操作目录, 下载安装脚本
  ```bash
    ## 创建操作目录
    cd ~ && mkdir -p fisco && cd fisco
    
    ## 下载脚本
    curl -#LO https://github.com/FISCO-BCOS/FISCO-BCOS/releases/download/v2.9.1/build_chain.sh && chmod u+x build_chain.sh
  ```
  ![image](https://github.com/shadowNo-1/FISCO-BCOS_Build_Tutorial/assets/61909905/1b17845b-2b1e-4b2a-881f-1318964d31ff)
 > [!NOTE]
 > 如果因为网络问题导致长时间无法下载`build_chain.sh`脚本，请尝试：<br />
 > ```bash
 > curl -#LO https://osp-1257653870.cos.ap-guangzhou.myqcloud.com/FISCO-BCOS/FISCO-BCOS/releases/v2.9.1/build_chain.sh && chmod u+x build_chain.sh
 > ```

- ### 第三步. 搭建单群组4节点联盟链
  在fisco目录下执行如下指令，生成一条单群组4节点的FISCO链。 请确保机器的`30300~30303`，`20200~20203`，`8545~8548`端口没有被占用。
  ```bash
  bash build_chain.sh -l 127.0.0.1:4 -p 30300,20200,8545
  ```
  :round_pushpin:[国密](https://fisco-bcos-documentation.readthedocs.io/zh-cn/latest/docs/manual/guomi_crypto.html)版本请执行：
  ```bash
  bash build_chain.sh -l 127.0.0.1:4 -p 30300,20200,8545 -g -G
  ```
  > 其中`-g`表示生成国密配置，`-G`表示使用国密SSL连接
  > 
  > [!NOTE]
  > - 其中-p选项指定起始端口，分别是p2p_port,channel_port,jsonrpc_port
  > - 出于安全性和易用性考虑，v2.3.0版本最新配置将listen_ip拆分成jsonrpc_listen_ip和channel_listen_ip，但仍保留对listen_ip的解析功能，详细请参考[这里](https://fisco-bcos-documentation.readthedocs.io/zh-cn/latest/docs/manual/configuration.html#rpc)














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
