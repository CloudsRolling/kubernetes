命令表
查看类命令：
获取节点相应服务的信息 ： kubectl get pods   按selector名来查找pod：     kubectl get pod --selector name=redis
查看集群信息：           kubectl cluster-info
查看各组件信息：         kubectl -s http://localhost:8080 get componentstatuses  或 kubectl get cs
查看pods所在的运行节点:  kubectl gkubectl get pods -o yamlet pods -o wide
查看pods定义的详细信息： kubectl get pods -o yaml
查看运行pod的环境变量：  kubectl exec pod名 env
查看指定pod的日志：      kubectl logs -f pods/heapster-xxxxx -n kube-system

操作类命令：  
创建资源：               kubectl apply -f 文件名.yaml   kubectl create -f 文件名.yaml
重建资源：               kubectl replace -f 文件名  [--force]
删除资源：               kubectl delete -f 文件名、kubectl delete pod pod名、kubectl delete rc rc名、kubectl delete service service名

kubectl进阶命令操作
kubectl get：获取指定资源的基本信息  kubectl get services kubernetes-dashboard -n kube-system #查看所有service
                                   kubectl get deployment kubernetes-dashboard -n kube-system #查看所有发布
                                   kubectl get pods --all-namespaces #查看所有pod
                                   kubectl get pods -o wide --all-namespaces #查看所有pod的IP及节点
                                   kubectl get pods -n kube-system | grep dashboard
                                   kubectl get nodes -l zone #获取zone的节点
kubectl describe：查看指定资源详细描述信息    kubectl describe service/kubernetes-dashboard --namespace="kube-system"
                                            kubectl describe pods/kubernetes-dashboard-349859023-g6q8c --namespace="kube-system" #指定类型查看
                                            kubectl describe pod nginx-772ai #查看pod详细信息
kubectl scale：动态伸缩               kubectl scale rc nginx --replicas=5 # 动态伸缩
                                     kubectl scale deployment redis-slave --replicas=5 #动态伸缩
                                     kubectl scale --replicas=2 -f redis-slave-deployment.yaml #动态伸缩
kubectl exec：进入pod启动的容器        kubectl exec -it redis-master-1033017107-q47hh /bin/bash #进入容器

kubectl label：添加label值            kubectl label nodes node1 zone=north #增加节点lable值 spec.nodeSelector: zone: north #指定pod在哪个节点
                                     kubectl label pod redis-master-1033017107-q47hh role=master #增加lable值 [key]=[value]
                                     kubectl label pod redis-master-1033017107-q47hh role- #删除lable值
                                     kubectl label pod redis-master-1033017107-q47hh role=backend --overwrite #修改lable值
                                     
kubectl rolling-update：滚动升级      kubectl rolling-update redis-master -f redis-master-controller-v2.yaml #配置文件滚动升级
                                     kubectl rolling-update redis-master --image=redis-master:2.0 #命令升级
                                     kubectl rolling-update redis-master --image=redis-master:1.0 --rollback #pod版本回滚
etcdctl 常用操作
                      etcdctl cluster-health #检查网络集群健康状态
                      etcdctl --endpoints=https://192.168.71.221:2379 cluster-health #带有安全认证检查网络集群健康状态
                      etcdctl member list
                      etcdctl set /k8s/network/config '{ "Network": "10.1.0.0/16" }'
                      etcdctl get /k8s/network/config 
    
