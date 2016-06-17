# 学习指南

本文主要围绕道里云SDN技术，介绍开发过程中用到的一些OpenFlow技术和开源产品，同时总结一下学习路线。

***使用到的开源软件:***

*1. Python & Golang*

*2. Docker*

*3. OpenvSwitch*

*4. OpenFlow & Ryu*

*5. DaoliNet*

## 1. Python & Golang

Python是一门简单易学的高级解释型语言，可以通过交互式命令行执行，也可以直接通过解释器执行Python文件.

Golang是一门新起的面向对象的高级编译型语言，需要通过go工具链生成可执行文件，执行效率高.

#### 1.1. Python/Golang语言学习

**1.1.1. Python/Golang的安装和环境的配置**

> Linux发行版系统默认已经安装Python语言环境，也可以在官网下载Python源代码重组编译安装Python(参考Python文档).
>
> Golang某些系统中可以通过安装包管理工具直接下载，也可以下载源代码编译安装，如下:
>
>       wget https://storage.googleapis.com/golang/go1.5.3.linux-amd64.tar.gz
>       tar xzvf go1.5.3.linux-amd64.tar.gz -C /usr/local/
>       export PATH=$PATH:/usr/local/go/bin

**1.1.2. Python/Golang语言和语法规则**

比如:

*Python*                              | *Golang*
------------------------------------- | ------------------------
Python以缩进来组织代码                | Golang以花括号来组织代码，并且花括号必须紧跟在一行最后
不需要在末尾加分号进行分行            | 不需要在末尾加分号进行分行
使用class来定义类，使用def来定义方法  | 使用type...struct定义类，使用func定义方法
使用try...catch来捕获异常             | 使用defer和recover捕获异常
使用\_\_init\_\_.py文件表示包         | 使用package关键字表示包

**1.1.3. 熟悉常用库函数和第三方库的使用**

***Python:*** os,sys,re,match,copy,time,types, struct,socket,cPickle,httplib

***Golang:*** fmt,io,bytes,strings,time, net/http,encoding/json

**1.1.4. 编写一些简单程序和网络应用程序**

***Python输出字符:***

    print "Python Print"

***Golang输出字符:***

    package main

    func main() {
        fmt.printf("Golang Print")
    }


**1.1.5. 阅讲习一些Python/Golang的开源产品**

***Python:*** Django,Ryu,OpenStack

***Golang:*** Docker,Kubenetes

#### 1.2. Python/Golang参考文档

***Python文档:*** <https://docs.python.org/2.7/tutorial/index.html>

***Golang文档:*** <http://docs.studygolang.com/doc/>

## 2. Docker

**2.1. 在服务器安装Docker软件**

    curl -fsSL https://get.docker.com/ | sh
    systemctl start docker

**2.2. 学会Docker命令行工具的使用**

    docker help查看命令行工具使用

比如:

    # 下载centos镜像
    docker pull centos
    # 启动名为test的容器
    docker run -d --name test centos

**2.3. 搭建swarm集群环境(或者k8s)**

参考：<https://docs.docker.com/swarm/get-swarm/>

**2.4. Docker插件的开发**

参考：<https://docs.docker.com/engine/extend/plugin_api/>

***Docker官方文档:*** <https://docs.docker.com/>

## 3. OpenvSwitch

**3.1. 安装OpenvSwitch**

*编译:*

    yum install -y openssl-devel rpm-build
    wget http://openvswitch.org/releases/openvswitch-2.5.0.tar.gz
    mkdir -p ~/rpmbuild/SOURCES
    cp openvswitch-2.5.0.tar.gz ~/rpmbuild/SOURCES/
    tar xzf openvswitch-2.5.0.tar.gz
    rpmbuild -bb --without check openvswitch-2.5.0/rhel/openvswitch.spec

*安装:*

    yum localinstall -y rpmbuild/RPMS/x86_64/openvswitch-2.5.0-1.x86_64.rpm

*启动:*

    /etc/init.d/openvswitch start


**3.2. 学会OpenvSwitch命令行工具的使用**

以下例子中BridgeName表示ovs网桥的名称.

    # 查看ovs版本
    ovs-vsctl --version

    # 添加ovs网桥
    ovs-vsctl add-br BridgeName

    # 添加端口
    ovs-vsctl add-port BridgeName eth0

    # 查看ovs网桥信息
    ovs-vsctl show

    # 添加流表
    ovs-ofctl add-flow BridgeName priority=10,in_port=1,icmp,action=drop

    # 删除流表
    ovs-ofctl del-flows BridgeName in_port=1,icmp

    # 查看流表
    ovs-ofctl dump-flows BridgeName

**3.3. Docker结合OpenvSwitch使用**

在DaoliNet中使用ovsplugin插件可以将Docker容器虚拟网卡挂接到ovs Bridge上.

也可以用pipework工具添加，参考<https://github.com/jpetazzo/pipework>.

**3.4. 与OpenFlow控制器结合**

*配置ovs连接OpenFlow控制器*

    ovs-vsctl set-controller BridgeName tcp:127.0.0.1:6633

***文档:*** <http://openvswitch.org/support/>

## 4. OpenFlow & Ryu

**4.1. OpenFlow常见控制器**

*JAVA实现: OpenDaylight, Floodlight*

*Python实现: ONOS, RYU, POX*

**4.2. Ryu控制器介绍**

*安装:*

    # 使用pip命令安装:
    pip install ryu

    # 或源码安装:
    git clone git://github.com/osrg/ryu.github
    cd ryu; python ./setup.py install

*运行:*

安装ryu完成后，会在python包路径(CentOS: /usr/lib/python2.7/site-packages/)存放源码，其中ryu目录下的app文件中是官方给出的一些事例，也可以参考例子实现更加复杂的OpenFlow应用。

例如, 运行简单mac地址学习功能的simple_switch.py应用:

    cd ryu/app/
    ryu-manager simple_switch.py

**4.3. 文档**

***OpenFlow文档:*** [OpenFlow规范](openflow-spec-v1.1.0.pdf)

***Ryu文档:*** <http://ryu.readthedocs.io/en/latest/>

## 5. DaoliNet

DaoliNet开源项目是结合以上OpenFlow技术和OpenvSwitch等开源软件实现的一套Docker容器网络解决方案，旨在使用轻量级OpenFlow技术解决跨Host容器网络互联和隔离，同时避免封包和路由协议引发的问题。

DaoliNet包括四个组件:

1. **[daolinet](https://github.com/daolinet/daolinet)** - DaoliNet API Service和DaoliNet Agent
2. **[daolicontroller](https://github.com/daolinet/daolicontroller)** - OpenFlow控制器结合
3. **[ovsplugin](https://github.com/daolinet/ovsplugin)** - 支持OpenvSwitch的Docker插件
4. **[daolictl](https://github.com/daolinet/daolictl)** - 类似docker的命令行工具

**项目地址:*** <https://github.com/daolinet/daolinet>

***DaoliNet文档:*** <https://github.com/daolinet/daolinet/tree/master/docs>
