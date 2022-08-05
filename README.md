# ek8-for-centos7-v1.24.3

[下载](https://github.com/catman002/kubernetes-ek8/releases)


# EK8说明：
```
Ek8[easy kubernetes]是一款快速安装和高可用性的kubernetes产品，简单易用 【v1.23.3及以上版本go语言重新实现】
只需通过简单配置集群服务器ip基础信息，即可通过一条命令即可完成k8s集群环境配置、安装。该产品具有以下特点：
1) 集群安装包主要由Kube-apiserver、kuber-controller-manager、kuber-scheduler、kuelet、coredns、calico、etcd、keepalived、haproxy、docker、containerd和registry组成
2) 支持灵活的安装类型。用户可以选择全部安装或选择性安装
3) 安装程序自动检查配置。包括IP合法性，服务器连通性、帐户可用性和IP可用性
4) 安装程序自动设置群集服务器环境所需的环境
```
- **下载ek8-for-{os}-{version}-on-{os}.tar.gz, 拷贝到安装机并解压**
- **安装前先配置 config.cfg 文件，设置相关服务器信息[注释为 “需修改“ 的是必须要修改]**
- **请确保ek8安装程序所在的计算机可以通过SSH登录集群服务器，并且各个服务器的root密码一致**
- **安装机支持centos和mac；集群服务器必须为centos7.x**

```
- 如果您需要支持k8s更高版本或支持其他服务器版本，请在GitHub上给作者留言

```

# 安装前须知、准备:
```
- 当前的ek8版本支持的集群服务器为CentOS7.x；安装机可以是 MAC 或者 centos7.x
- 初次安装需要产品key：123456 
- 安装过充中需要提供集群服务器登录密码，根据提示输入即可
- 安装前，先在安装机安装必要软件：shpass和rsync
- 初次安装会检查集群中服务器的内核版本，低于4.18会自动升级、升级完后会提示重启服务器，根据提示重新执行安装命令即可！！！
- kubernetes1.24.x及以上版本容器用的是containerd，如需docker容器，请下载v1.23.x版本
```

# 安装步骤：
```
1)修改config.cfg，配置服务器相关信息
2)ek8 qinstall all，开始安装集群(初次执行需要提供产品秘锁：123456；然后根据提示输入远程服务器的ssh的root密码)
```

# 常用命令：
```
- ek8 help 查看命令帮助
- ek8 qinstall  all 安装集群环境所需的所有组件 (全新安装采用，是原create+install命令的组合，建议采用)
- ek8 qinstall  all  --exclude=docker,etcd 安装集群环境所需的所有组件,但无需重新安装docker和etcd
- ek8 qinstall kubelet,kubeproxy 重新安装kubelet和kubeproxy
- ek8 qdelete all 删除集群环境所有组件和缓存（containerd/docker和registry缓存）
- ek8 delete all 删除集群环境所有组件和缓存
- ek8 appendnodes all 快速部署新的节点服务器 （v1.24.x及以上支持,用于后期增加node快速加入集群，需要配置new_node_servers=(nodeX:xx.xx.xx.xx)参数）
- ek8 appendmasters all 快速部署新的master节点服务器 v1.24.x及以上支持,用于后期增加master快速加入集群,需要配置new_api_servers=(nodeX:xx.xx.xx.xx)参数）
- ek8 stop/start [mod] 停止/启动集群特定服务
... ...
```

#安装完成后的一些操作需要注意的
- 1.24.x默认容器为containerd,安装完后，可以采用 ctr/crictl/nerdctl 操作
- ctr/nerdctl 默认的的image空间为 default,crictl默认image的操作空间是k8s.io，需要注意
- kubernetes内部默认采用的是crictl，所以一定要记得它的image空间位置是k8s.io !!!!!!!

构建image可以采用nerdctl build -t xxx .

