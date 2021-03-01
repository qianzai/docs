# 9. Kubernetes 集群调度

## 9.1. 调度器 简介

`Scheduler `是 `kubernetes `的调度器。 调度 是指将 Pod 放置到合适的 Node 上，然后对应 Node 上的 Kubelet 才能够运行这些 pod。

**需要满足一下原则：**

- 公平：如何保证每个节点都能被分配资源
- 资源高效利用：集群所有资源最大化被使用
- 效率：调度的性能要好，能够尽快地对大批量的 pod 完成调度工作
- 灵活：允许用户根据自己的需求控制调度的逻辑

`Sheduler `是作为单独的程序运行的，启动之后会一直监听  **API Server**，获取PodSpec.NodeName为空的 pod，对每个 pod 都会创建一个 binding，表明该 pod 应该放到哪个节点。



## 9.2. 调度流程

**kube-scheduler** 给一个 `pod `做调度选择包含两个步骤：

1. 过滤
2. 打分

过滤阶段会将所有满足 `Pod `调度需求的 `Node `选出来。这个过程称为***predicate***。然后调度器会为 `Pod `从所有可调度节点中选取一个最合适的 `Node`。根据当前启用的打分规则，调度器会给每一个可调度节点进行打分。这个过程称为***Priorities***。



**predicate有一系列的算法可以使用：**

- `PodFitsResources`：节点上剩余的资源是否大于pod请求的资源
- `PodFitsHost`：如果pod指定了NodeName，坚持节点名称是否和NodeName匹配
- `PodFitsHostPorts`：节点上已经使用的port是否和pod申请的port冲突
- `PodSelectorMatches`：过滤掉和pod指定的label不匹配的节点
- `NoDiskConflict`：已经mount的volume和pod指定的volume不冲突，除非他们只是只读

如果在predicate过程中没有合适的节点，pod会一直在pending状态，不断重试调度，直到有节点满足条件。经过这个步骤，如果有多个节点满足条件，就继续priorities过程，按照优先级大小对节点进行排序



***priority***优先级 由一系列键值对组成，键是该优先级项的名称，值是他的权重（该项的重要性）。这些优先级包括：

- `LeastRequestedPriority`：通过计算CPU和Memory的使用率来决定权重，使用率越低权重越高，换句话说这个优先级指标倾向于资源使用比例更低的节点
- `BalancedResourceAllocation`：节点上CPU和Memory使用率接近，权重越高。这个应该和上面的一起使用，不应该单独使用
- `ImageLocalityPriority`：倾向于已经有要使用镜像的节点，镜像总大小值越大，权重越高



## 9.3. 亲和性

### 9.3.1. 节点亲和性

**节点亲和性调：**:**pod.spec.nodeAffinity**

- preferredDuringSchedulingIgnoredDuringExecution：软策略
- requiredDuringSchedulingIgnoredDuringExecution：硬策略

> 软策略就是希望满足，不满足也没事。硬策略就是一定要满足，不满足不行。



#### 9.3.1.1. 键值运算关系

- In：label 的值在某个列表中 
- NotIn：label 的值不在某个列表中 
- Gt：label 的值大于某个值 
- Lt：label 的值小于某个值 
- Exists：某个 label 存在 
- DoesNotExist：某个 label 不存在 

**注意：**

如果``nodeselectorTerms`下面有多个选项的话，满足任何一个条件就可以了;如果`matchExpressions`有多个选项的话，则必须同时满足这些条件才能正常调度 POD



#### 9.3.1.2. 演示案例

`requiredDuringSchedulingIgnoredDuringExecution`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: affinity
  labels:
    app: node-affinity-pod
spec:
  containers:
  - name: with-node-affinity
    image: qianzai/k8s-myapp:v1
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: kubernetes.io/hostname
            operator: NotIn
            values:
            - k8s-node02
```

> 不在`k8s-node02` 中

```shell
[root@k8s-master01 ~]# kubectl get node --show-labels 
NAME           STATUS   ROLES    AGE   VERSION   LABELS
k8s-master01   Ready    master   26d   v1.15.1   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/arch=amd64,kubernetes.io/hostname=k8s-master01,kubernetes.io/os=linux,node-role.kubernetes.io/master=
k8s-node01     Ready    <none>   26d   v1.15.1   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/arch=amd64,kubernetes.io/hostname=k8s-node01,kubernetes.io/os=linux
k8s-node02     Ready    <none>   26d   v1.15.1   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/arch=amd64,kubernetes.io/hostname=k8s-node02,kubernetes.io/os=linux
```

> `kubernetes.io/hostname=k8s-node01`，可以发现每个node都有一个默认的键名键值对。

```shell
[root@k8s-master01 affi]# kubectl create -f pod1.yaml 
pod/affinity created
[root@k8s-master01 affi]# kubectl get pod -o wide
NAME       READY   STATUS    RESTARTS   AGE   IP            NODE         NOMINATED NODE   READINESS GATES
affinity   1/1     Running   0          13s   10.244.1.62   k8s-node01   <none>           <none>
```

> 可以发现 ，无论创建多少次都在 node1 节点。



将节点亲和性改为 在k8s-node03 中

```yaml
 affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: kubernetes.io/hostname
            operator: In
            values:
            - k8s-node03
```

> k8s-node03 节点是不存在的

```shell
[root@k8s-master01 affi]# kubectl delete pod --all && kubectl create -f  pod1.yaml 
pod "affinity" deleted
pod/affinity created
[root@k8s-master01 affi]# kubectl get pod 
NAME       READY   STATUS    RESTARTS   AGE
affinity   0/1     Pending   0          8s
```

> 没有合适的节点，pod会一直在pending状态



`preferredDuringSchedulingIgnoredDuringExecution`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: affinity
  labels:
    app: node-affinity-pod
spec:
  containers:
  - name: with-node-affinity
    image: qianzai/k8s-myapp:v1
  affinity:
    nodeAffinity:
      preferredDuringSchedulingIgnoredDuringExecution:
      - weight: 1		# 权重
        preference:
          matchExpressions:
          - key: kubernetes.io/hostname
            operator: In
            values:
            - k8s-node03
```

```shell
[root@k8s-master01 affi]# kubectl delete pod --all && kubectl create -f  pod2.yaml 
pod "affinity" deleted
pod/affinity created
[root@k8s-master01 affi]# kubectl get pod -o wide
NAME       READY   STATUS    RESTARTS   AGE    IP            NODE         NOMINATED NODE   READINESS GATES
affinity   1/1     Running   0          11s   10.244.1.65   k8s-node01   <none>           <none>
```

> 可以发现，即使没有 k8s-node03 ，还是可以 被调度到 其他 节点。

将节点亲和性改为 在k8s-node02 中，然后再运行

```shell
[root@k8s-master01 affi]# vim pod2.yaml 
[root@k8s-master01 affi]# kubectl delete pod --all && kubectl create -f  pod2.yaml 
pod "affinity" deleted
pod/affinity created
[root@k8s-master01 affi]# kubectl get pod -o wide
NAME       READY   STATUS    RESTARTS   AGE   IP            NODE         NOMINATED NODE   READINESS GATES
affinity   1/1     Running   0          6s    10.244.2.65   k8s-node02   <none>           <none>
```

> 可以发现，k8s-node02 存在，就一定会被调度到 k8s-node02中





### 9.3.2. Pod亲和性

**pod.spec.affinity.podAffinity/podAntiAffinity**

- preferredDuringSchedulingIgnoredDuringExecution：软策略
- requiredDuringSchedulingIgnoredDuringExecution：硬策略



#### 9.3.2.1. topologyKey 拓扑域

顾名思义，`topology` 就是 `拓扑` 的意思，这里指的是一个 `拓扑域`，是指一个范围的概念，比如一个 Node、一个机柜、一个机房或者是一个地区（如杭州、上海）等，实际上对应的还是 Node 上的标签。这里的 `topologyKey`对应的是 Node 上的标签的 Key（没有Value），可以看出，其实 `topologyKey` 就是用于筛选 Node 的。通过这种方式，我们就可以将各个 Pod 进行跨集群、跨机房、跨地区的调度了。

**比如：**

如果使用 `k8s.io/hostname`，则表示拓扑域为 Node 范围，那么 `k8s.io/hostname` 对应的值不一样就是不同的拓扑域。比如 Pod1 在 `k8s.io/hostname=node1` 的 Node 上，Pod2 在 `k8s.io/hostname=node2` 的 Node 上，Pod3 在 `k8s.io/hostname=node1` 的 Node 上，则 Pod2 和 Pod1、Pod3 不在同一个拓扑域，而Pod1 和 Pod3在同一个拓扑域。

如果使用 `failure-domain.k8s.io/zone` ，则表示拓扑域为一个区域。同样，Node 的标签 `failure-domain.k8s.io/zone` 对应的值不一样也不是同一个拓扑域，比如 Pod1 在 `failure-domain.k8s.io/zone=beijing`的 Node 上，Pod2 在 `failure-domain.k8s.io/zone=hangzhou` 的 Node 上，则 Pod1 和 Pod2 不属于同一个拓扑域。



#### 9.3.2.2. 演示案例

`requiredDuringSchedulingIgnoredDuringExecution`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: node01
  labels:
    app: node01
spec:
  containers:
  - name: with-node-affinity
    image: qianzai/k8s-myapp:v1

---
apiVersion: v1
kind: Pod
metadata:
  name: pod-3
  labels:
    app: pod-3
spec:
  containers:
  - name: pod-3
    image: qianzai/k8s-myapp:v1
  affinity:
    podAffinity:	
      requiredDuringSchedulingIgnoredDuringExecution:
      - labelSelector:
          matchExpressions:
          - key: app
            operator: In
            values:
            - node01
        topologyKey: kubernetes.io/hostname
```

```shell
[root@k8s-master01 affi]# kubectl create -f pod3.yaml 
pod/node01 created
pod/pod-3 created
[root@k8s-master01 affi]# kubectl get pod -o wide
NAME     READY   STATUS    RESTARTS   AGE   IP            NODE         NOMINATED NODE   READINESS GATES
node01   1/1     Running   0          9s    10.244.1.67   k8s-node01   <none>           <none>
pod-3    1/1     Running   0          9s    10.244.1.66   k8s-node01   <none>           <none>

[root@k8s-master01 affi]# kubectl delete pod --all && kubectl create -f pod3.yaml
pod "node01" deleted
pod "pod-3" deleted
pod/node01 created
pod/pod-3 created
[root@k8s-master01 affi]# kubectl get pod -o wide
NAME     READY   STATUS    RESTARTS   AGE   IP            NODE         NOMINATED NODE   READINESS GATES
node01   1/1     Running   0          3s    10.244.2.67   k8s-node02   <none>           <none>
pod-3    1/1     Running   0          3s    10.244.2.66   k8s-node02   <none>           <none>
```

> 可以发现，pod-3，一定和node01 处于同一 拓扑域

将 podAﬃnity 修改为 podAnitAﬃnity，然后运行

```shell
[root@k8s-master01 affi]# vim pod3.yaml 
[root@k8s-master01 affi]# kubectl delete pod --all && kubectl create -f pod3.yaml
No resources found
pod/node01 created
pod/pod-3 created
[root@k8s-master01 affi]# kubectl get pod -o wide
NAME     READY   STATUS    RESTARTS   AGE   IP            NODE         NOMINATED NODE   READINESS GATES
node01   1/1     Running   0          12s   10.244.1.74   k8s-node01   <none>           <none>
pod-3    1/1     Running   0          12s   10.244.2.69   k8s-node02   <none>           <none>
```

> 可以发现 ，一定不在同一 拓扑域

`软策略就与之前类似，也很好理解`



---



**亲和性/反亲和性调度策略比较如下：**

| **调度策略**  | **匹配标签** | **操作符**                                | **拓扑域支持** | **调度目标**               |
| ------------- | ------------ | ----------------------------------------- | -------------- | -------------------------- |
| nodeAﬃnity    | 主机         | In, NotIn, Exists,   DoesNotExist, Gt, Lt | 否             | 指定主机                   |
| podAﬃnity     | POD          | In, NotIn, Exists,   DoesNotExist         | 是             | POD与指定POD同一拓扑域     |
| podAnitAﬃnity | POD          | In, NotIn, Exists,   DoesNotExist         | 是             | POD与指定POD不在同一拓扑域 |



## 9.4. 污点和容忍度

**节点亲和性** 是 `Pod `的一种属性，它使 Pod 被吸引到一类特定的节点。 这可能出于一种偏好，也可能是硬性要求。 `Taint`（污点）则相反，它使节点能够排斥一类特定的 Pod。

**容忍度**（`Tolerations`）是应用于 `Pod `上的，允许（但并不要求）`Pod `调度到带有与之匹配的污点的节点上。

**污点和容忍度**（Toleration）相互配合，可以用来避免 `Pod `被分配到不合适的节点上。 每个节点上都可以应用一个或多个污点，这表示对于那些不能容忍这些污点的 `Pod`，是不会被该节点接受的。



### 9.4.1. 污点(Taint)

使用`kubectl taint`命令可以给某个 `Node `节点设置污点，`Node `被设置上污点之后就和 `Pod `之间存在了一种相斥的关系，可以让 `Node `拒绝 `Pod `的调度执行，甚至将 `Node `已经存在的 `Pod `驱逐出去

**每个污点的组成如下：**

`key=value:effect`

每个污点有一个 `key `和 `value `作为污点的标签，其中 `value `可以为空，`effect `描述污点的作用。当前 `tainteffect `支持如下三个选项：

- **NoSchedule**：表示 k8s 将不会将 Pod 调度到具有该污点的 Node 上
- **PreferNoSchedule**：表示 k8s 将尽量避免将 Pod 调度到具有该污点的 Node 上
- **NoExecute**：表示 k8s 将不会将 Pod 调度到具有该污点的 Node 上，同时会将 Node 上已经存在的 Pod 驱逐出去

```shell
 命令 kubectl taint 给节点增加一个污点。比如
kubectl taint nodes node1 key1=value1:NoSchedule
```

> 给节点 `node1` 增加一个污点，它的键名是 `key1`，键值是 `value1`，效果是 `NoSchedule`。 这表示只有拥有和这个污点相匹配的容忍度的 Pod 才能够被分配到 `node1` 这个节点。

```shell
 若要移除上述命令所添加的污点，你可以执行：
kubectl taint nodes node1 key:NoSchedule-
```

```shell
 节点说明中，查找 Taints 字段
kubectl describe pod  pod-name  

[root@k8s-master01 ~]# kubectl describe nodes k8s-master01 |grep Taints
Taints:             node-role.kubernetes.io/master:NoSchedule
```

> 比如 `master`节点，默认就打上了 `NoSchedule`



### 9.4.2. 容忍(Tolerations)

设置了污点的 `Node `将根据 `taint `的 **effect：NoSchedule、PreferNoSchedule、NoExecute** 和 `Pod `之间产生互斥的关系，`Pod `将在一定程度上不会被调度到 `Node `上。但我们可以在 `Pod `上设置容忍 ( `Toleration `) ，意思是设置了容忍的 `Pod `将可以容忍污点的存在，可以被调度到存在污点的 `Node `上。

```shell
 分别给两个节点，打上污点。
kubectl taint node k8s-node02 check=bzm:NoExecute
kubectl taint node k8s-node02 check=bzm:NoExecute
```

`tolerations.yaml`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-t
  labels:
    app: pod-t
spec:
  containers:
    - name: pod-t
      image: qianzai/k8s-myapp:v1
  tolerations:
    - key: "check"
      operator: "Equal"
      value: "bzm"
      effect: "NoExecute"
      tolerationSeconds: 3600
```

```shell
[root@k8s-master01 ~]# vim tolerations.yaml
[root@k8s-master01 ~]# kubectl create -f tolerations.yaml 
pod/pod-t created
[root@k8s-master01 ~]# kubectl get pod -o wide
NAME    READY   STATUS    RESTARTS   AGE   IP            NODE         NOMINATED NODE   READINESS GATES
pod-t   1/1     Running   0          20s   10.244.1.76   k8s-node01   <none>           <none>
```

> 即使有**污点**，设置了**容忍** ，依然可以运行。



## 9.5. 固定节点

**指定调度节点**

1、Pod.spec.nodeName 将 Pod 直接调度到指定的 Node 节点上，会跳过 Scheduler 的调度策略，该匹配规则是强制匹配

```yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: myweb
spec:
  replicas: 7
  template:
    metadata:
      labels:
        app: myweb
    spec:
      nodeName: k8s-node01
      containers:
      - name: myweb
        image:  qianzai/k8s-myapp:v1
        ports:
        - containerPort: 80
```

> 根据节点名称选择

```shell
[root@k8s-master01 ~]# kubectl create -f pod1.yaml 
deployment.extensions/myweb created
[root@k8s-master01 ~]# kubectl get pod -o wide
NAME                     READY   STATUS    RESTARTS   AGE   IP            NODE         NOMINATED NODE   READINESS GATES
myweb-695f7fb9c9-fpcjl   1/1     Running   0          11s   10.244.1.78   k8s-node01   <none>           <none>
...
...
myweb-695f7fb9c9-wtx8q   1/1     Running   0          11s   10.244.1.80   k8s-node01   <none>           <none>
```

> 七个pod ，都运行在了pod节点

2、Pod.spec.nodeSelector：通过 kubernetes 的 label-selector 机制选择节点，由调度器调度策略匹配 label，而后调度 Pod 到目标节点，该匹配规则属于强制约束

```yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: myweb
spec:
  replicas: 2
  template:
    metadata:
      labels:
        app: myweb
    spec:
      nodeSelector:
        disk: ssd
      containers:
      - name: myweb
        image: qianzai/k8s-myapp:v1
        ports:
        - containerPort: 80
```

> disk=ssd标签匹配

```shell
[root@k8s-master01 ~]# kubectl create -f pod2.yaml 
deployment.extensions/myweb created
[root@k8s-master01 ~]# kubectl get pod -o wide
NAME                     READY   STATUS    RESTARTS   AGE   IP       NODE     NOMINATED NODE   READINESS GATES
myweb-858c5d996f-n5fnv   0/1     Pending   0          7s    <none>   <none>   <none>           <none>
myweb-858c5d996f-qn8cp   0/1     Pending   0          7s    <none>   <none>   <none>           <none>
[root@k8s-master01 ~]# kubectl label nodes k8s-node01 disk=ssd
node/k8s-node01 labeled
[root@k8s-master01 ~]# kubectl get pod -o wide
NAME                     READY   STATUS    RESTARTS   AGE   IP            NODE         NOMINATED NODE   READINESS GATES
myweb-858c5d996f-n5fnv   1/1     Running   0          44s   10.244.1.85   k8s-node01   <none>           <none>
myweb-858c5d996f-qn8cp   1/1     Running   0          44s   10.244.1.86   k8s-node01   <none>           <none>
```

> 指定调度到 disk=ssd 的节点之上

```shell
 给k8s-node02也打上标签，并修改副本数目

[root@k8s-master01 ~]# kubectl label nodes k8s-node02 disk=ssd
node/k8s-node02 labeled  
[root@k8s-master01 ~]# kubectl edit deployments myweb 
deployment.extensions/myweb edited
[root@k8s-master01 ~]# kubectl get pod -o wide
NAME                     READY   STATUS    RESTARTS   AGE   IP            NODE         NOMINATED NODE   READINESS GATES
myweb-858c5d996f-272cq   1/1     Running   0          66s   10.244.2.75   k8s-node02   <none>           <none>
myweb-858c5d996f-2pvh2   1/1     Running   0          66s   10.244.1.90   k8s-node01   <none>           <none>
myweb-858c5d996f-b2fhf   1/1     Running   0          66s   10.244.1.92   k8s-node01   <none>           <none>
myweb-858c5d996f-dmj4s   1/1     Running   0          66s   10.244.2.77   k8s-node02   <none>           <none>
myweb-858c5d996f-jl9h6   1/1     Running   0          66s   10.244.1.89   k8s-node01   <none>           <none>
myweb-858c5d996f-jnc4q   1/1     Running   0          66s   10.244.2.76   k8s-node02   <none>           <none>
myweb-858c5d996f-n8rlb   1/1     Running   0          66s   10.244.2.78   k8s-node02   <none>           <none>
myweb-858c5d996f-rckf4   1/1     Running   0          66s   10.244.1.91   k8s-node01   <none>           <none>
```

> node01和node02都能运行