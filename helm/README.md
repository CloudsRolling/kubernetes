helm 可以理解为 Kubernetes 的包管理工具，可以方便地发现、共享和使用为Kubernetes构建的应用。

一、基本概念
1.Helm的三个基本概念Chart：Helm应用（package），包括该应用的所有Kubernetes manifest模版，
类似于YUM RPM或Apt dpkg文件Repository：Helm package存储仓库Release：chart的部署实例，每个chart可以部署一个或多个release

2.Helm工作原理：Helm包括两个部分，helm客户端和tiller服务端。
the client is responsible for managing charts, and the server is responsible for managing releases.

3.helm客户端helm客户端是一个命令行工具，负责管理charts、reprepository和release。
它通过gPRC API（使用kubectl port-forward将tiller的端口映射到本地，然后再通过映射后的端口跟tiller通信）向tiller发送请求，
并由tiller来管理对应的Kubernetes资源。Helm客户端的使用方法参见Helm命令

4.tiller服务端tiller接收来自helm客户端的请求，并把相关资源的操作发送到Kubernetes，负责管理（安装、查询、升级或删除等）和跟踪Kubernetes资源。
为了方便管理，tiller把release的相关信息保存在kubernetes的ConfigMap中。tiller对外暴露gRPC API，供helm客户端调用。

