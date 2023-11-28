# FISCO-BCOS_Build_Tutorial

How to Construct [FISCO-BCOS](https://github.com/FISCO-BCOS/FISCO-BCOS) Consortium Chain and Common Errors

This is [shadowNo-1](https://github.com/shadowNo-1)'s private note

[![Author](https://img.shields.io/badge/author-shadowNo--1-informational?logo=github)](https://github.com/shadowNo-1)
![](https://img.shields.io/badge/Ubuntu-22.04.3-informational?style=flat&logo=ubuntu&logoColor=e95420&color=e95420)

![](https://img.shields.io/badge/language-Solidity-informational?style=flat&logo=solidity&logoColor=9b96be&color=2b247c)
![](https://img.shields.io/badge/license-MIT-informational?style=flat&logo=conventionalcommits&logoColor=fe5196&color=fe5196)
![](https://img.shields.io/badge/FISCO--BCOS-v2.x-informational?style=flat&logo=<LOGO_NAME>&logoColor=white&color=194ea0)

## Badge Gather
![](https://img.shields.io/badge/GNU-2.0-informational?style=flat&logo=gnu&logoColor=white&color=A42E2B)
![](https://img.shields.io/badge/license-MIT-informational?style=flat&logo=conventionalcommits&logoColor=FE5196&color=FE5196)
![](https://img.shields.io/badge/JavaScript-2.0-informational?style=flat&logo=javascript&logoColor=F7DF1E&color=F7DF1E)
![](https://img.shields.io/badge/VueJS-2.0-informational?style=flat&logo=vuedotjs&logoColor=4FC08D&color=4FC08D)
![](https://img.shields.io/badge/Debian-12-informational?style=flat&logo=debian&logoColor=A81D33&color=A81D33)
![](https://img.shields.io/badge/Ethereum-2.0-informational?style=flat&logo=ethereum&logoColor=white&color=3C3C3D)
![](https://img.shields.io/badge/Bootstrap-5.3.2-informational?style=flat&logo=bootstrap&logoColor=B486F9&color=7952B3)

![](https://img.shields.io/badge/<WORD_ON_LEFT>-<WORD_ON_RIGHT>-informational?style=flat&logo=<LOGO_NAME>&logoColor=white&color=2bbc8a)
## FISCO-BCOS 版本说明
FISCO-BCOS目前主要存在[2.x](https://fisco-bcos-documentation.readthedocs.io/zh_CN/latest/)和[3.x](https://fisco-bcos-doc.readthedocs.io/zh-cn/latest/)两个大版本，用户可以根据不同的场景选择:

·v2.x与v3.x版本互不兼容，但后续会同时保持维护、迭代、更新。

·v2.x是目前的稳定版本，已经经过多个机构、多个应用，长时间在生产环境中的实践检验，具备金融级的高性能、高可用性及高安全性。该版本会持续进行维护。用户当前有生产级的使用需求，可以直接使用v2.x即可。

·v3.x为当前FISCO-BCOS最新的版本，采用微服务的架构，已发布正式版。

## 基础环境配置

将source.list复制为source.list.bak备份
sudo cp /etc/apt/sources.list /etc/apt/sources.list_bak


### 国内源替换
选择对应版本的源替换，详见清华源[help](https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/)


## 搭建第一个区块链网络


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
