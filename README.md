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

## :information_source:本文所用实例系统环境配置
```bash
----------------------------------------------------------------------
 CPU Model            : 11th Gen Intel(R) Core(TM) i5-11260H @ 2.60GHz
 CPU Cores            : 4 Cores 2611.211 MHz x86_64
 CPU Cache            : 12288 KB 
 OS                   : Ubuntu 22.04.3 LTS (64 Bit) VMware
 Kernel               : 6.2.0-37-generic
 Total Space          : 13.2 GB / 20.7 GB 
 Total RAM            : 952 MB / 3871 MB (1142 MB Buff)
 Total SWAP           : 0 MB / 2139 MB
 Uptime               : 0 days 0 hour 4 min
 Load Average         : 0.11, 0.12, 0.05
 TCP CC               : cubic
----------------------------------------------------------------------
```
![image](https://github.com/shadowNo-1/FISCO-BCOS_Build_Tutorial/assets/61909905/63b9e851-f6d8-4fb4-be90-84e5d93cd014)


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


# ![image](https://github.com/shadowNo-1/FISCO-BCOS_Build_Tutorial/blob/main/img/FISCO_BCOS.svg)搭建第一个区块链网络
[![FISCO--BCOS](https://is.gd/71u78p)](https://fisco-bcos-documentation.readthedocs.io/zh_CN/latest/)
![](https://img.shields.io/badge/Solidity-0.4.24-informational?style=flat&logo=solidity&logoColor=9B96BE&color=2B247C)
![](https://img.shields.io/badge/Ubuntu-22.04.3-informational?style=flat&logo=ubuntu&logoColor=e95420&color=e95420)
![Java](https://is.gd/QQflSA)

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
  📥安装net-tools网络管理工具
  ```bash
  sudo apt install net-tools
  ```
  🚨执行下列命令，检查机器的`30300~30303`，`20200~20203`，`8545~8548`端口是否被占用
  ```bash
  netstat -an | grep tcp
  ```
  在fisco目录下执行如下指令，生成一条单群组4节点的FISCO链。 请确保机器的`30300~30303`，`20200~20203`，`8545~8548`端口没有被占用。
  ```bash
  bash build_chain.sh -l 127.0.0.1:4 -p 30300,20200,8545
  ```
  🔔[国密](https://fisco-bcos-documentation.readthedocs.io/zh-cn/latest/docs/manual/guomi_crypto.html)版本请执行：
  ```bash
  bash build_chain.sh -l 127.0.0.1:4 -p 30300,20200,8545 -g -G
  ```
  > 其中`-g`表示生成国密配置，`-G`表示使用国密SSL连接
  
<br />

  > [!NOTE]
  > - 其中-p选项指定起始端口，分别是p2p_port,channel_port,jsonrpc_port
  > - 出于安全性和易用性考虑，v2.3.0版本最新配置将listen_ip拆分成jsonrpc_listen_ip和channel_listen_ip，但仍保留对listen_ip的解析功能，详细请参考[配置RPC](https://fisco-bcos-documentation.readthedocs.io/zh-cn/latest/docs/manual/configuration.html#rpc)
  > - 为便于开发和体验，channel_listen_ip参考配置是 0.0.0.0 ，出于安全考虑，请根据实际业务网络情况，修改为安全的监听地址，如：内网IP或特定的外网IP

  命令执行成功会输出`All completed`。如果执行出错，请检查`nodes/build.log`文件中的错误信息。
  ![image](https://github.com/shadowNo-1/FISCO-BCOS_Build_Tutorial/assets/61909905/128e54dc-2b49-4501-b65d-45aa7c81a02d)

  - ### 第四步. 启动FISCO BCOS链
    ▶️启动所有节点
    ```bash
    bash nodes/127.0.0.1/start_all.sh
    ```
    启动成功会输出类似下面内容的响应。否则请使用`netstat -an | grep tcp`检查机器的`30300~30303`，`20200~20203`，`8545~8548`端口是否被占用。
    
    ![image](https://github.com/shadowNo-1/FISCO-BCOS_Build_Tutorial/assets/61909905/ca2bee08-b491-4376-9d98-715074a432eb)

 - ### 第五步. 检查进程
   🧐检查进程是否启动
   ```bash
   ps -ef | grep -v grep | grep fisco-bcos
   ```
   ⬇️正常情况会输出类似以下的4个进程； 如果进程数缺失，则是进程启动失败（通常是端口被占用导致的）
   ![image](https://github.com/shadowNo-1/FISCO-BCOS_Build_Tutorial/assets/61909905/4b026536-2cf1-4446-bda2-006249791cfd)
 - ### 第六步. 检查日志输出
   - 执行下列指令，查看节点node0链接的节点数
     ```bash
     tail -f nodes/127.0.0.1/node0/log/log*  | grep connected
     ```
     通常情况下会不停地输出连接信息，从输出的信息可以看出node0与另外3个节点有连接。<br />
     ⛔按`Ctrl`+`C`终止输出<br />
   
     ![image](https://github.com/shadowNo-1/FISCO-BCOS_Build_Tutorial/assets/61909905/6805e0a3-b914-4f40-9fef-4d14d17c9f18)<br />
   
   - 执行下面指令，检查是否在共识
     ```bash
     tail -f nodes/127.0.0.1/node0/log/log*  | grep +++
     ```
     正常情况会不停输出带有`++++Generating seal`的日志，即表示共识正常。<br />
     ⛔按`Ctrl`+`C`终止输出<br />
     
     ![image](https://github.com/shadowNo-1/FISCO-BCOS_Build_Tutorial/assets/61909905/5e3b378f-6c77-4d82-9932-04321a834170)<br />
     <br />
     
## 2. 配置及使用控制台
   > [!IMPORTANT]
   > - `console 1.x`系列基于[Web3SDK](https://fisco-bcos-documentation.readthedocs.io/zh-cn/latest/docs/sdk/java_sdk.html)实现，`console 2.6`之后基于[JavaSDK](https://fisco-bcos-documentation.readthedocs.io/zh-cn/latest/docs/sdk/java_sdk/index.html)实现，最新版本控制台基于 `JavaSDK` 实现
   > - 2.6及其以上版本控制台使用文档请参考[2.6+ docs](https://fisco-bcos-documentation.readthedocs.io/zh-cn/latest/docs/console/console_of_java_sdk.html) ，1.x版本控制台使用文档请参考[1.x docs](https://fisco-bcos-documentation.readthedocs.io/zh-cn/latest/docs/console/console.html)
   > - 可通过命令`./start.sh --version`查看当前控制台版本
   > - 基于[Web3SDK](https://fisco-bcos-documentation.readthedocs.io/zh-cn/latest/docs/sdk/java_sdk.html)开发应用时将**Solidity**代码转换为**Java**代码时，必须使用**1.x**版本控制台，具体请参考[控制台使用说明](https://fisco-bcos-documentation.readthedocs.io/zh-cn/latest/docs/console/download_console.html)

   在控制台链接**FISCO BCOS**节点，实现查询**区块链状态、部署调用合约**等功能，能够快速获取到所需要的信息。2.6版本控制台指令详细介绍参考[2.6+ docs](https://fisco-bcos-documentation.readthedocs.io/zh-cn/latest/docs/console/console_of_java_sdk.html)，1.x版本控制台指令详细介绍参考[1.x docs](https://fisco-bcos-documentation.readthedocs.io/zh-cn/latest/docs/console/console.html)。

   - ### 第一步. 准备依赖
     - 📥安装Java （推荐使用Java 14）。
       ```bash
       # ubuntu系统安装java
       sudo apt install -y default-jdk
       
       #centos系统安装java
       sudo yum install -y java java-devel
       ```
       
     - 获取控制台并回到fisco目录
       ```bash
       cd ~/fisco && curl -LO https://github.com/FISCO-BCOS/console/releases/download/v2.9.2/download_console.sh && bash download_console.sh
       ```
       🔔如果因为网络问题导致长时间无法下载，请尝试：
       ```bash
       cd ~/fisco && curl -#LO https://gitee.com/FISCO-BCOS/console/raw/master-2.0/tools/download_console.sh && bash download_console.sh
       ```
       
     - 拷贝控制台配置文件
     
       🚨若节点未采用默认端口，请将文件中的20200替换成节点对应的channel端口。
       ```bash
       # 最新版本控制台使用如下命令拷贝配置文件
       cp -n console/conf/config-example.toml console/conf/config.toml
       ```

     - 配置控制台证书
       > ⚠️使用***1.x***版本控制台时：<br />
       > 🚨搭建***国密版***时，如果使用国密SSL请执行：<br />
       > ```bash
       > cp nodes/127.0.0.1/sdk/gm/* console/conf/
       > ```
       > 🚨搭建***国密版***时，请修改`applicationContext.xml`中`encryptType`修改为`1` <br />
    
        ```bash
        cp -r nodes/127.0.0.1/sdk/* console/conf/
        ```
       ![image](https://github.com/shadowNo-1/FISCO-BCOS_Build_Tutorial/assets/61909905/245e307a-f954-44cb-b029-38fa08eb6670)
   - ### 第二步. 启动并使用控制台
     - 启动控制台
        ```bash
        cd ~/fisco/console && bash start.sh
        ```
       输出**Welcome to FISCO BCOS console!**及相关信息表明启动成功，否则请检查`conf/config.toml`中**节点端口**配置是否正确
       ![image](https://github.com/shadowNo-1/FISCO-BCOS_Build_Tutorial/assets/61909905/976bda4d-451a-4ebe-b185-aa8110821929) <br />
       🔔若**1.x**控制台启动失败，参考[Web3SDK启动失败场景](https://fisco-bcos-documentation.readthedocs.io/zh-cn/latest/docs/faq/connect.html)
     - 用控制台获取信息
       
       📜获取客户端版本
       ```bash
       getNodeVersion
       ```
       ![image](https://github.com/shadowNo-1/FISCO-BCOS_Build_Tutorial/assets/61909905/9e948e56-a518-418b-849d-d792b1caea20)
       
       📜获取节点信息
       ```bash
       getPeers
       ```
       ![image](https://github.com/shadowNo-1/FISCO-BCOS_Build_Tutorial/assets/61909905/a61e327c-590f-4c21-8bc5-16b2339ef8f1)

       📄全部输出结果：
       ```bash
       =============================================================================================
       Welcome to FISCO BCOS console(2.9.2)!
       Type 'help' or 'h' for help. Type 'quit' or 'q' to quit console.
        ________ ______  ______   ______   ______       _______   ______   ______   ______  
       |        |      \/      \ /      \ /      \     |       \ /      \ /      \ /      \ 
       | $$$$$$$$\$$$$$|  $$$$$$|  $$$$$$|  $$$$$$\    | $$$$$$$|  $$$$$$|  $$$$$$|  $$$$$$\
       | $$__     | $$ | $$___\$| $$   \$| $$  | $$    | $$__/ $| $$   \$| $$  | $| $$___\$$
       | $$  \    | $$  \$$    \| $$     | $$  | $$    | $$    $| $$     | $$  | $$\$$    \ 
       | $$$$$    | $$  _\$$$$$$| $$   __| $$  | $$    | $$$$$$$| $$   __| $$  | $$_\$$$$$$\
       | $$      _| $$_|  \__| $| $$__/  | $$__/ $$    | $$__/ $| $$__/  | $$__/ $|  \__| $$
       | $$     |   $$ \\$$    $$\$$    $$\$$    $$    | $$    $$\$$    $$\$$    $$\$$    $$
        \$$      \$$$$$$ \$$$$$$  \$$$$$$  \$$$$$$      \$$$$$$$  \$$$$$$  \$$$$$$  \$$$$$$
       
       =============================================================================================
       [group:1]> getNodeVersion  # 获取客户端版本
       ClientVersion{
           version='2.9.1',
           supportedVersion='2.9.1',
           chainId='1',
           buildTime='20220922 08:57:35',
           buildType='Linux/g++/Release',
           gitBranch='HEAD',
           gitCommitHash='83a87ad749475c0edcc6d5ce2dabd328a36d3bae'
       }
       
       [group:1]> getPeers  # 获取节点信息
       [
           PeerInfo{
               nodeID='aa605d5f61b457dc4e9519066534f0ebb2e801fd44694a31dab2bf8e8ae583527d5001d27430886d50db923d5c99ac4b911bad07f477844323b03228dfbae0b6',
               iPAndPort='127.0.0.1:30301',
               node='node1',
               agency='agency',
               topic='[
                   _block_notify_1
               ]'
           },
           PeerInfo{
               nodeID='de606df6456ad9a168835c7b28300bb6301fea1b94bfd7eb8d4d9a3c335df2fc6925a254cbb7b60b806b06ac3d6efa7d6612208e7be3b86573f24c684464c99b',
               iPAndPort='127.0.0.1:30303',
               node='node3',
               agency='agency',
               topic='[
                   
               ]'
           },
           PeerInfo{
               nodeID='11703407d4960298f1ef21bd53d366801b05baa39d576475d0c9c7902c28ddf7914c92c1f052a1b4db50a8a35c6ed2e1579e825fa155931529cb32ce10866fce',
               iPAndPort='127.0.0.1:30302',
               node='node2',
               agency='agency',
               topic='[
                   
               ]'
           }
       ]
       
       [group:1]>
       ```
## 3. 部署及调用HelloWorld合约

   - ### 第一步. 编写HelloWorld合约
     HelloWorld合约提供两个接口，分别是`get()`和`set()`，用于获取/设置合约变量`name`。合约内容如下:
     ```solidity
     pragma solidity ^0.4.24;
     
     contract HelloWorld {
         string name;
     
         function HelloWorld() {
             name = "Hello, World!";
         }
     
         function get()constant returns(string) {
             return name;
         }
     
         function set(string n) {
             name = n;
         }
     }
     Copy to clipboard
   - ### 第二步. 部署HelloWorld合约
     为了方便用户快速体验，HelloWorld合约已经内置于控制台中，位于控制台目录下`contracts/solidity/HelloWorld.sol`。
     
     ⌨️在控制台输入以下指令，部署成功则返回合约地址
     ```bash
     deploy HelloWorld
     ```
     ![image](https://github.com/shadowNo-1/FISCO-BCOS_Build_Tutorial/assets/61909905/c4a49a86-0c5e-4206-9475-1c8508e61de1)

   - ### 第三步. 调用HelloWorld合约
     🔎查看当前块高
     ```bash
     getBlockNumber
     ```
     ![image](https://github.com/shadowNo-1/FISCO-BCOS_Build_Tutorial/assets/61909905/1a37223b-4e3f-408e-b8e2-7846ba7afbeb)

     🔃调用get接口获取name变量，此处的**合约地址**是deploy指令返回的地址
     ```bash
     # 合约地址使用deploy返回的地址，不要使用案例地址
     call HelloWorld 0xc769e9877fbab23ecf93aee74e6b06e2602392d8 get
     ```

     🔎查看当前块高，块高不变，因为get接口不更改账本状态
     ```bash
     getBlockNumber
     ```
     ![image](https://github.com/shadowNo-1/FISCO-BCOS_Build_Tutorial/assets/61909905/7ca7e01e-f5d6-42ba-a6ce-2fdb057097e3)


     📝调用set设置name
     ```bash
     # 合约地址使用deploy返回的地址，不要使用案例地址
     call HelloWorld 0xc769e9877fbab23ecf93aee74e6b06e2602392d8 set "Hello, FISCO BCOS"
     ```
     ![image](https://github.com/shadowNo-1/FISCO-BCOS_Build_Tutorial/assets/61909905/3922907a-d216-4907-94e4-71852faed1a6)

     🔎再次查看当前块高，块高增加表示已出块，账本状态已更改
     ```bash
     getBlockNumber
     ```
     ![image](https://github.com/shadowNo-1/FISCO-BCOS_Build_Tutorial/assets/61909905/fe918d7b-084b-4085-a80b-9e16de56fc27)

     🔃调用get接口获取name变量，检查设置是否生效
     ```bash
     # 合约地址使用deploy返回的地址，不要使用案例地址
     call HelloWorld 0xc769e9877fbab23ecf93aee74e6b06e2602392d8 get
     ```
     ![image](https://github.com/shadowNo-1/FISCO-BCOS_Build_Tutorial/assets/61909905/e4303eda-1c99-4086-978e-5b14e5cf225d)

     调用get接口获取name变量，检查设置是否生效
     ```bash
     quit
     ```

     输出综合
     ```bash
     [group:1]> getBlockNumber  # 查看当前块高
     1
     
     [group:1]> call HelloWorld 0xc769e9877fbab23ecf93aee74e6b06e2602392d8 get  # 调用get接口获取name变量 此处的合约地址是deploy指令返回的地址
     ---------------------------------------------------------------------------------------------
     Return code: 0
     description: transaction executed successfully
     Return message: Success
     ---------------------------------------------------------------------------------------------
     Return value size:1
     Return types: (STRING)
     Return values:(Hello, World!)
     ---------------------------------------------------------------------------------------------
     
     [group:1]> getBlockNumber  # 查看当前块高，块高不变，因为get接口不更改账本状态
     1
     
     [group:1]> call HelloWorld 0xc769e9877fbab23ecf93aee74e6b06e2602392d8 set "Hello, FISCO BCOS"  # 调用set设置name
     transaction hash: 0x3fa73b2ef1daf616838b12bef8868e1358cdd3af930e4e9341e69b6e2095cc09
     ---------------------------------------------------------------------------------------------
     transaction status: 0x0
     description: transaction executed successfully
     ---------------------------------------------------------------------------------------------
     Transaction inputs:
     Input value size:1
     Input types: (STRING)
     Input values:(Hello, FISCO BCOS)
     ---------------------------------------------------------------------------------------------
     Receipt message: Success
     Return message: Success
     Return values:[]
     ---------------------------------------------------------------------------------------------
     Event logs
     Event: {}
     
     [group:1]> getBlockNumber  # 再次查看当前块高，块高增加表示已出块，账本状态已更改
     2
     
     [group:1]> call HelloWorld 0xc769e9877fbab23ecf93aee74e6b06e2602392d8 get  # 调用get接口获取name变量，检查设置是否生效
     ---------------------------------------------------------------------------------------------
     Return code: 0
     description: transaction executed successfully
     Return message: Success
     ---------------------------------------------------------------------------------------------
     Return value size:1
     Return types: (STRING)
     Return values:(Hello, FISCO BCOS)
     ---------------------------------------------------------------------------------------------
     
     [group:1]> quit  # 退出控制台
     ```



## ⏰最后编辑日期
   2023-11-30 T 16:46:08 GMT+8
   
## 📚Reference and Citation
   [清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/) <br />
   [中科大开源软件镜像站](https://mirrors.ustc.edu.cn/) <br />
   [浙江大学开源软件镜像站](https://mirrors.zju.edu.cn/) <br />
   [Ethereum Development Documentation](https://ethereum.org/en/developers/docs/) <br />
   [FISCO BCOS技术文档](https://fisco-bcos-documentation.readthedocs.io/zh-cn/latest/index.html) <br />

## 📜License
   ![](https://img.shields.io/badge/license-GNU-informational?style=flat&logo=gnu&logoColor=white&color=A42E2B)
   
   GNU General Public License v3.0
   
   Please see [LICENSE](https://github.com/shadowNo-1/FISCO-BCOS_Build_Tutorial/blob/main/LICENSE) for details.






##
##
##
##


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
