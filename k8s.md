# Kubernetes 学习

## Kubernetes说明
 
Kubernetes是一个开源系统，用于管理多个主机上的容器化应用程序;提供应用程序部署，维护和扩展的基本机制。

Kubernetes建立在Google使用Borg系统大规模运行生产工作负载的十五年经验的基础上，结合了社区中的最佳创意和实践。

## Kubernetes解决的问题： 
1. 调度 - 容器应该在哪个机器上运行 
2. 生命周期和健康状况 - 容器在无错的条件下运行 
3. 服务发现 - 容器在哪，怎样与它通信 
4. 监控 - 容器是否运行正常 
5. 认证 - 谁能访问容器 
6. 容器聚合 - 如何将多个容器合并成一个工程

## Kubernetes组件组成： 
1. kubectl 
客户端命令行工具，将接受的命令格式化后发送给kube-apiserver，作为整个系统的操作入口。 
2. kube-apiserver 
作为整个系统的控制入口，以REST API服务提供接口。 
3. kube-controller-manager 
用来执行整个系统中的后台任务，包括节点状态状况、Pod个数、Pods和Service的关联等。 
4. kube-scheduler 
负责节点资源管理，接受来自kube-apiserver创建Pods任务，并分配到某个节点。 
5. etcd 
负责节点间的服务发现和配置共享。 
6. kube-proxy 
运行在每个计算节点上，负责Pod网络代理。定时从etcd获取到service信息来做相应的策略。 
7. kubelet 
运行在每个计算节点上，作为agent，接受分配该节点的Pods任务及管理容器，周期性获取容器状态，反馈给kube-apiserver。 
8. DNS 
一个可选的DNS服务，用于为每个Service对象创建DNS记录，这样所有的Pod就可以通过DNS访问服务了。

## Kubernetes两种建立方式：

##### You have a working [Go environment].

```
$ go get -d k8s.io/kubernetes
$ cd $GOPATH/src/k8s.io/kubernetes
$ make
```

##### You have a working [Docker environment].

```
$ git clone https://github.com/kubernetes/kubernetes
$ cd kubernetes
$ make quick-release
```

For the full story, head over to the [developer's documentation].


## Kubernetes目录

Kubernetes源码分析从cmd包进行分析:cmd作为Kubernetes主入口，源码分析比较容易点，kubernetes代码量带大了，很多都是在pkg包中的。

* ../src/k8s.io/kubernetes/cmd/kubelet/kubelet.go

   中 Kubernetes cmd kubelet [kubelet.md](cmd/kubelet/kubelet.md)
* 

*

*

*

*

*

*


================================================================
#### 部分资料来自网络

[announcement]: https://cncf.io/news/announcement/2015/07/new-cloud-native-computing-foundation-drive-alignment-among-container
[Borg]: https://research.google.com/pubs/pub43438.html
[CNCF]: https://www.cncf.io/about
[communication]: https://git.k8s.io/community/communication
[community repository]: https://git.k8s.io/community
[containerized applications]: https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/
[developer's documentation]: https://git.k8s.io/community/contributors/devel#readme
[Docker environment]: https://docs.docker.com/engine
[Go environment]: https://golang.org/doc/install
[GoDoc]: https://godoc.org/k8s.io/kubernetes
[GoDoc Widget]: https://godoc.org/k8s.io/kubernetes?status.svg
[interactive tutorial]: http://kubernetes.io/docs/tutorials/kubernetes-basics
[kubernetes.io]: http://kubernetes.io
[Scalable Microservices with Kubernetes]: https://www.udacity.com/course/scalable-microservices-with-kubernetes--ud615
[Submit Queue]: http://submit-queue.k8s.io/#/ci
[Submit Queue Widget]: http://submit-queue.k8s.io/health.svg?v=1
[troubleshooting guide]: https://kubernetes.io/docs/tasks/debug-application-cluster/troubleshooting/

