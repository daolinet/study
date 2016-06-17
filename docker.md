# Docker技术浅谈

伴随着云计算的兴起，Docker容器是近两年发展比较快，知名度比较高的热门技术。无论是对开发测试人员，还是对于云应用，Docker容器技术在提升效率，节省资源，快速交付等方面都表现出其强大的优势，并且在大大小小的场景中使用。

### Docker技术的发展过程

**Linux容器(lxc)**

Linux Container技术通过Linux内核中cgroups子系统的进程和资源隔离特性，提供轻量级虚拟化功能。LXC提供一个用户空间工具集让用户管理Lxc容器并为容器分配资源。

**Docker容器**

Linux容器很早就存在于Linux系统中，但是一直都处于少数应用状态。当Docker出现后，容器技术又重新被审视，Docker提供了更加友好的API接口和容器管理方式，为开发者和应用者提供了焕然一新的体验方式。Docker最早后端还是依赖于Lxc容器技术，随后实现了Docker又实现libcontainer来管理容器，不依赖lxc和其它包，直接操作容器的namespace，cgroups等系统。

Docker在发展过程中，其功能越来越完善，代码结构越来越清析。Docker后端网络驱动最早都集成在libcontainer中，到后来实现了libnetwork功能，将网络功能单独组织，并且在最新版本中实现了容器跨主机overlay网络功能。同时Docker以插件方式为开发者提供volume和Network的扩展能力。

随着Docker容器技术的发展，同时也催生出为容器优化的Coreos系统，为容器提供编排和调试服务的Kubernetes，Mesos等，为容器提供网络解决方案的Weave, Flannel，Calico, DaoliNet等，还有以Docker提供Paas云平台解决方案。之后, Linux基金会成立OCI(Open Container Initiative)，围绕容器格式和运行时制定一个开放的工业化标准。


### Docker的编排技术

**Swarm**

Docker Swarm是Docker官方提供的一个容器集群管理和调度的工具，它将集群中所有Docker Host池化为单个虚拟Docker Host，由于Swarm使用标准的Docker API，任何其它调用Docker API的应用都可以直接使用Docker Swarm。

**Kubernetes**

Kubernetes是Google开源的容器集群管理系统，为容器应用提供资源调度、冗余备份、服务发现、扩容缩容等整一套功能，可以做为容器Paas平台使用。

**Mesos**

Mesos是一个分布式集群资源管理和调度的框架，通过抽象主机的CPU、内存、存储等计算资源来提供一套高效，容错，弹性的分布式系统。它支持包括Docker容器任务，Hadoop/Spark计算任务等。

**OpenStack**

云管理平台OpenStack最早通过nova-docker插件支持docker作为后端虚拟化驱动，随后又通过云编排组件Heat支持docker，由于docker和k8s的成熟，又实现了Magnum来集成docker的k8s，再结合heat来部署容器应用。

对于不同编排技术有不同的应用场景:

- Swarm作为Docker本地支持的集群管理工具，只支持Docker容器应用，实现了高可用，弹性，调度和服务发现等功能。

- Kubernetes适合作为大规模环境使用，与Swarm功能类似，但又对容器做了更加细化控制，同时可定制化使用。

- Mesos不是专为容器设计，除了可以管理Docker容器外，更重要是在高性能计算中混合环境中使用。

- OpenStack自身做为Iaas提供服务，Docker容器可以做为其它后端虚拟机驱动，也可以做为应用编排服务。

### Docker应用

软件集成和交付

传统的软件开发测试交付过程中，通过chef，puppet等自动化工具进行，在使用过程中需要用户熟悉这些工具的使用方式和配置选项，而Docker的build,ship,run以一种方便简洁的管理方式替代传统使用自动化工具方式。开发人员完成开发后，编写Dockerfile文件构建docker镜像，再上传到镜像库中，测试人员在本机下载镜像文件启动应用容器，全部过程只需要掌握docker工具的使用即可，从而降低了开发运维的复杂度。同时docker镜像采用tag方式管理，也能很好的进行版本控制工作。

Paas平台

在云计算时代，Docker容器作为Paas平台的一种替代方案，提供了完善的API来简化Paas应用的开发和管理，同时也出现许多开源的容器编排项目，极大推进了Paas的应用。
