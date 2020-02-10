# Cluster-Management
Direction and updates about nvidia computing cluster
# 新服务器集群下Docker的使用方法
相比较于本次升级之前的版本，本次升级主要特点为可管理性的增强。具体来说，一些对使用者重要的变化有以下几点：
- 三个计算结点以及管理节点之间的home目录是共享的。
- 三个计算节点以及管理节点之间的用户会自动同步。对于用户来说，也就是你的同一用户账号可以登录所有的节点。
- 新增了module模块，用来管理环境变量。在使用之前一般需要先加载自己需要用的module，以更改调用包的时候的路径。
- 计算节点不直接对外开放，只能通过先登录管理节点，在从管理节点ssh到计算节点。
- 新增slurm模块，用于计算资源的调度。该模块不是必要的。如果不掉用该模块，那么当前计算节点上的所有计算资源都是可见的。可根据自己需要选择s是否使用。
- 新增桌面图形化支持。
	
在进入了某一计算节点之后，基本可以参照之前的《服务器使用说明》来使用。建议在往下阅读之前先阅读《集群使用指南-东南大学》和《东南大学-V1》文档。下文将具体介绍如何在这些计算节点上使用Docker服务。

## Step 1：登陆服务器集群</br><br />
本次升级提供了和普通CentOS系统一样的图形化桌面，因此有多种方法来远程登录服务器集群（不在校园网上要配置好东南大学VPN）。
- **方法1：**
通过在浏览器输入10.129.4.17:300登陆服务器的远程桌面，之后根据提示操作即可（也可以利用其他第三方软件）。第一次登录时需要进行一定的设置（可参考《集群使用指南-东南大学》）。之后可以和普通的CentOS系统一样使用了。
注意此时登陆的是管理节点。由于计算节点不直接对外开放，因此我们需要通过管理节点来登录计算节点。在远程桌面上打开Terminal（Ctrl+Alt+T，或者通过左上角的Application找到Terminal），输入以下指令即可使用当前用户登录计算节点。
```
ssh <计算节点IP或hostname>
```
例如`ssh 10.129.10.1`或`ssh compute01`。目前可用的计算节点有3个，分别是compute01, compute02, compute03。对应的IP分别为10.129.10.1, 10.129.10.2, 10.129.10.3。</br><br />
- **方法2：**
在命令行输入以下指令来登录
```
ssh <用户名>@<IP>
```
例如`ssh cuiyiming@10.129.4.17`。输入密码即可登录。登陆成功后输入以下指令即可登录计算节点。
```
ssh <计算节点IP或hostname>
```
例如`ssh 10.129.10.1`或`ssh compute01`即可使用当前账户ssh登录计算节点。</br><br />
- **方法3：**
利用其他第三方软件实现类似方法1和2中的服务。

## Step 2:配置docker环境</br><br />
Docker集群版v18.09.8已经事先安装在compute01-03这三个节点上了。输入以下指令来加载Docker服务环境。
```
module load docker/18.09.8 
```
docker服务已经事先在三个服务器上启动好了。输入指令`docker ps`，如命令执行成功，说明docker环境和docker服务均已成功加载。可以把之前服务器上备份下来的docker镜像压缩包通过scp指令（可参考《集群使用指南-东南大学》）传输到服务器上，并重新加载镜像（可参考《nvidia-docker使用说明》）。之后即可按照之前的方法使用docker服务了。
