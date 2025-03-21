1、namespace
```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: dev
```

2、service
```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-svc
  namespace: dev
spec:
  selector:
    app: nginx-pod
  ports:
    - port: 80
      nodePort: 30000
      targetPort: 80
```

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-svc
  namespace: dev
spec:
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80
  type: ClusterIP
  clusterIP: 10.109.179.231
```

3、deployment
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deploy
  namespace: dev
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx-container
          image: nginx
          ports:
            - name: nginx-port
              containerPort: 80
              protocol: TCP
```

4、pod
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  namespace: dev
  labels:
    app: nginx-pod
spec:
  containers:
    - name: nginx-pod
      image: nginx
      ports:
        - name: nginx-port
          containerPort: 80
          protocol: TCP
```

5、示例
```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: dev
 
---
 
apiVersion: v1
kind: Service
metadata:
  name: nginxsvc
  namespace: dev
spec:
  selector:
    app: nginx-pod
  type: NodePort
  ports:
    - port: 80
      nodePort: 30000
      targetPort: 80
 
---
 
apiVersion: v1
kind: Pod
metadata:
  name: nginxpod
  namespace: dev
  labels:
    app: nginx-pod
spec:
  containers:
    - name: nginx-containers
      image: nginx
      ports:
        - name: nginx-port
          containerPort: 80
          protocol: TCP
```

```bash
# 创建资源：
# kubectl apply -f  nginx_pod.yaml
# kubectl create -f  nginx_pod.yaml
# kubectl delete -f  nginx_pod.yaml
 
# kubectl get po -n dev
# 查看详情：kubectl describe po nginxpod -n dev
# kubectl run nginx --image=nginx -n dev dryrun -oyaml
# 进入容器：kubectl exec -it nginxpod -n dev -- /bin/bash
 
# 查看：
# kubectl get ns default -o yaml
# kubectl get pods -n dev -o wide
# kubectl get po -n dev
# kubectl get svc -n dev
# kubectl get deploy -n dev
 
# 创建：
# kubectl create ns dev
 
# 删除：
# kubectl delete ns dev
 
 
# 标签
# 添加：kubectl label pod nginxpod version=2.0 -n dev
# 更新：kubectl label pod nginxpod version=2.0 -n dev --overwrite
# 查看：kubectl get po nginxpod -n dev --show-labels
# 筛选：kubectl get po -n dev -l version=2.0 --show-labels
# 删除：kubectl label pod nginxpod -n dev version-
 
 
# deployment
# kubectl create deployment 名称 【参数】
# kubectl create deploy  nginx --image=nginx:latest --port=80 --replicas=3 -n dev
# kubectl get deploy -n dev -owide
# kubectl get po -n dev
# kubectl describe deploy nginx -n dev
# kubectl delete deploy nginx -n dev
# kubectl create -f deploy-nginx.yaml
# kubectl delete -f deploy-nginx.yaml
 
 
# 暴露服务：expose service
# 集群访问：kubectl expose deploy nginx --name=nginx-svc --type=ClusterIP --port=80 --target-port=80 -n dev
# 外部访问：kubectl expose deploy nginx --name=nginx-svc --type=NodePort --port=80 --target-port=80 -n dev
# kubectl get svc nginx-svc -n dev -owide
 
 
# 查看文档
# kubectl explain 资源类型.属性
```

6、pause根容器


7、pod
```yaml
apiVersion: v1     #必选，版本号，例如v1
kind: Pod       　 #必选，资源类型，例如 Pod
metadata:       　 #必选，元数据
  name: string     #必选，Pod名称
  namespace: string  #Pod所属的命名空间,默认为"default"
  labels:       　　  #自定义标签列表
    - name: string      　          
spec:  #必选，Pod中容器的详细定义
  containers:  #必选，Pod中容器列表
  - name: string   #必选，容器名称
    image: string  #必选，容器的镜像名称
    imagePullPolicy: [ Always|Never|IfNotPresent ]  #获取镜像的策略 
    command: [string]   #容器的启动命令列表，如不指定，使用打包时使用的启动命令
    args: [string]      #容器的启动命令参数列表
    workingDir: string  #容器的工作目录
    volumeMounts:       #挂载到容器内部的存储卷配置
    - name: string      #引用pod定义的共享存储卷的名称，需用volumes[]部分定义的的卷名
      mountPath: string #存储卷在容器内mount的绝对路径，应少于512字符
      readOnly: boolean #是否为只读模式
    ports: #需要暴露的端口库号列表
    - name: string        #端口的名称
      containerPort: int  #容器需要监听的端口号
      hostPort: int       #容器所在主机需要监听的端口号，默认与Container相同
      protocol: string    #端口协议，支持TCP和UDP，默认TCP
    env:   #容器运行前需设置的环境变量列表
    - name: string  #环境变量名称
      value: string #环境变量的值
    resources: #资源限制和请求的设置
      limits:  #资源限制的设置
        cpu: string     #Cpu的限制，单位为core数，将用于docker run --cpu-shares参数
        memory: string  #内存限制，单位可以为Mib/Gib，将用于docker run --memory参数
      requests: #资源请求的设置
        cpu: string    #Cpu请求，容器启动的初始可用数量
        memory: string #内存请求,容器启动的初始可用数量
    lifecycle: #生命周期钩子
        postStart: #容器启动后立即执行此钩子,如果执行失败,会根据重启策略进行重启
        preStop: #容器终止前执行此钩子,无论结果如何,容器都会终止
    livenessProbe:  #对Pod内各容器健康检查的设置，当探测无响应几次后将自动重启该容器
      exec:       　 #对Pod容器内检查方式设置为exec方式
        command: [string]  #exec方式需要制定的命令或脚本
      httpGet:       #对Pod内个容器健康检查方法设置为HttpGet，需要制定Path、port
        path: string
        port: number
        host: string
        scheme: string
        HttpHeaders:
        - name: string
          value: string
      tcpSocket:     #对Pod内个容器健康检查方式设置为tcpSocket方式
         port: number
       initialDelaySeconds: 0       #容器启动完成后首次探测的时间，单位为秒
       timeoutSeconds: 0    　　    #对容器健康检查探测等待响应的超时时间，单位秒，默认1秒
       periodSeconds: 0     　　    #对容器监控检查的定期探测时间设置，单位秒，默认10秒一次
       successThreshold: 0
       failureThreshold: 0
       securityContext:
         privileged: false
  restartPolicy: [Always | Never | OnFailure]  #Pod的重启策略
  nodeName: <string> #设置NodeName表示将该Pod调度到指定到名称的node节点上
  nodeSelector: obeject #设置NodeSelector表示将该Pod调度到包含这个label的node上
  imagePullSecrets: #Pull镜像时使用的secret名称，以key：secretkey格式指定
  - name: string
  hostNetwork: false   #是否使用主机网络模式，默认为false，如果设置为true，表示使用宿主机网络
  volumes:   #在该pod上定义共享存储卷列表
  - name: string    #共享存储卷名称 （volumes类型有很多种）
    emptyDir: {}       #类型为emtyDir的存储卷，与Pod同生命周期的一个临时目录。为空值
    hostPath: string   #类型为hostPath的存储卷，表示挂载Pod所在宿主机的目录
      path: string      　　        #Pod所在宿主机的目录，将被用于同期中mount的目录
    secret:       　　　#类型为secret的存储卷，挂载集群与定义的secret对象到容器内部
      scretname: string  
      items:     
      - key: string
        path: string
    configMap:         #类型为configMap的存储卷，挂载预定义的configMap对象到容器内部
      name: string
      items:
      - key: string
        path: string
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  namespace: dev
spec:
  restartPolicy: Never # 重启策略
  containers:
    - name: nginx
      image: nginx:latest
      imagePullPolicy: Never # 拉取策略
      command: ["/bin/bash","-c","date"] # 命令
      env: # 环境变量
        - name: "username"
          value: "admin"
        - name: "password"
          value: "123456"
      ports: # 容器端口
        - name: nginx-port
          containerPort: 80
          protocol: TCP
      resources: # 资源配额
        requests:
          cpu: 100m
          memory: 100Mi
        limits:
          cpu: 200m
          memory: 200Mi
      initContainers: # 初始化容器
        - name: test-redis
          image: busybox:1.30
          command: ["/bin/bash","-c","date"]
      lifecycle:
        postStart: # 钩子函数：启动后/停止前
          exec:
            command: ["/bin/bash","-c","date"]
        preStop:
          httpGet:
            scheme: HTTP
            host: 127.0.0.1
            port: 80
            path: /
      livenessProbe: # 存活探针
        tcpSocket:
          port: 8080
      readinessProbe: # 就绪探针
        httpGet:
          scheme: http
          host: 127.0.0.1
          port: 80
          path: /
        initalDelaySeconds: 30 # 开始探测
        timeoutSeconds: 5 # 探测超时
```

8、pod控制器
```
在kubernetes中，有很多类型的pod控制器，每种都有自己的适合的场景，常见的有下面这些：

ReplicationController：比较原始的pod控制器，已经被废弃，由ReplicaSet替代
ReplicaSet：保证副本数量一直维持在期望值，并支持pod数量扩缩容，镜像版本升级
Deployment：通过控制ReplicaSet来控制Pod，并支持滚动升级、回退版本
Horizontal Pod Autoscaler：可以根据集群负载自动水平调整Pod的数量，实现削峰填谷
DaemonSet：在集群中的指定Node上运行且仅运行一个副本，一般用于守护进程类的任务
Job：它创建出来的pod只要完成任务就立即退出，不需要重启或重建，用于执行一次性任务
Cronjob：它创建的Pod负责周期性任务控制，不需要持续后台运行
StatefulSet：管理有状态应用
```

```
版本回退：

deployment支持版本升级过程中的暂停、继续功能以及版本回退等诸多功能，下面具体来看.

kubectl rollout： 版本升级相关功能，支持下面的选项：

status 显示当前升级状态
history 显示 升级历史记录
pause 暂停版本升级过程
resume 继续已经暂停的版本升级过程
restart 重启版本升级过程
undo 回滚到上一级版本（可以使用–to-revision回滚到指定版本
```

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-rs
  namespace: dev
spec:
  revisionHistoryLimit: 3
  paused: false
  progressDeadlineSeconds: 600
  strategy: # 策略
    type: RollingUpdate
    rollingUpdate:
      maxSurage: 30%
      maxUnavailable: 30%
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx-container
          image: nginx:latest
          ports:
            - name: nginx-port
              containerPort: 80
```

```bash
# kubectl get rs -n dev -owide
# kubectl get po -n dev -owide
# kubectl get deployment,pod,svc -n dev
 
# kubectl scale rs nginx-rs --replicas=5 -n dev
# kubectl set image rs nginx-rs nginx-container=nginx:1.17.1 -n dev
 
# kubectl create rs nginx-rs -n dev
# kubectl edit rs nginx-rs -n dev
# kubectl delete rs nginx-rs -n dev --cascade=true
 
# 升级观察：kubectl get pods -n dev -w
# kubectl create -f deployment.yaml --record=true
 
 
 
# ----------------------------------------------------------------
# 版本回退
# 状态：kubectl rollout status deploy nginx-deploy -n dev
# 历史记录：kubectl rollout history deploy nginx-deploy -n dev
# 回滚：kubectl rollout undo deploy nginx-deploy -n dev --to-reversion=2
# 查看：kubectl get deploy -n dev -owide
# 暂停：kubectl rollout pause deploy nginx-deploy -n dev
# 恢复：kubectl rollout resume deploy nginx-deploy -n dev
 
# kubectl set image deploy pc-deployment nginx=nginx:1.17.4 -n dev && kubectl rollout pause deployment pc-deployment  -n dev
 
 
 
# ----------------------------------------------------------------
# hpa水平扩展
```

9、hpa水平扩展
```yaml
apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata:
  name: pc-hpa
  namespace: dev
spec:
  minReplicas: 1  #最小pod数量
  maxReplicas: 10 #最大pod数量
  targetCPUUtilizationPercentage: 3 # CPU使用率指标
  scaleTargetRef:   # 指定要控制的nginx信息
    apiVersion:  /v1
    kind: Deployment
    name: nginx
```

```bash
# kubectl create -f pc-hpa.yaml
# kubectl get hpa -n dev
# kubectl get deployment -n dev -w
```

10、DaemonSet（DS）
```yaml
apiVersion: batch/v1 # 版本号
kind: Job # 类型       
metadata: # 元数据
  name: # rs名称 
  namespace: # 所属命名空间 
  labels: #标签
    controller: job
spec: # 详情描述
  completions: 1 # 指定job需要成功运行Pods的次数。默认值: 1
  parallelism: 1 # 指定job在任一时刻应该并发运行Pods的数量。默认值: 1
  activeDeadlineSeconds: 30 # 指定job可运行的时间期限，超过时间还未结束，系统将会尝试进行终止。
  backoffLimit: 6 # 指定job失败后进行重试的次数。默认是6
  manualSelector: true # 是否可以使用selector选择器选择pod，默认是false
  selector: # 选择器，通过它指定该控制器管理哪些pod
    matchLabels:      # Labels匹配规则
      app: counter-pod
    matchExpressions: # Expressions匹配规则
      - {key: app, operator: In, values: [counter-pod]}
  template: # 模板，当副本数量不足时，会根据下面的模板创建pod副本
    metadata:
      labels:
        app: counter-pod
    spec:
      restartPolicy: Never # 重启策略只能设置为Never或者OnFailure
      containers:
      - name: counter
        image: busybox:1.30
        command: ["bin/sh","-c","for i in 9 8 7 6 5 4 3 2 1; do echo $i;sleep 2;done"]
```

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: nginx-ds
  namespace: dev
spec:
  revisionHistoryLimit: 3
  updateStrategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx-container
          image: nginx:1.17.1
 
 
# kubectl create -f nginx-daemon.yaml
# kubectl get ds -n dev -owide
# kubectl get po -n dev -owide
```

11、Job
```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: nginx-job
  namespace: dev
spec:
  manualSelector: true
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      restartPolicy: Never
      containers:
        - name: nginx-container
          image: nginx:1.17.1
 
 
# kubectl create -f nginx-job.yaml
# kubectl delete -f nginx-job.yaml
# kubectl get job -n dev -owide -w
# kubectl get po -n dev
```

12、CronJob(CJ)
```yaml
apiVersion: batch/v1beta1 # 版本号
kind: CronJob # 类型       
metadata: # 元数据
  name: # rs名称 
  namespace: # 所属命名空间 
  labels: #标签
    controller: cronjob
spec: # 详情描述
  schedule: # cron格式的作业调度运行时间点,用于控制任务在什么时间执行
  concurrencyPolicy: # 并发执行策略，用于定义前一次作业运行尚未完成时是否以及如何运行后一次的作业
  failedJobHistoryLimit: # 为失败的任务执行保留的历史记录数，默认为1
  successfulJobHistoryLimit: # 为成功的任务执行保留的历史记录数，默认为3
  startingDeadlineSeconds: # 启动作业错误的超时时长
  jobTemplate: # job控制器模板，用于为cronjob控制器生成job对象;下面其实就是job的定义
    metadata:
    spec:
      completions: 1
      parallelism: 1
      activeDeadlineSeconds: 30
      backoffLimit: 6
      manualSelector: true
      selector:
        matchLabels:
          app: counter-pod
        matchExpressions: 规则
          - {key: app, operator: In, values: [counter-pod]}
      template:
        metadata:
          labels:
            app: counter-pod
        spec:
          restartPolicy: Never 
          containers:
          - name: counter
            image: busybox:1.30
            command: ["bin/sh","-c","for i in 9 8 7 6 5 4 3 2 1; do echo $i;sleep 20;done"]
```

```
需要重点解释的几个选项：
schedule: cron表达式，用于指定任务的执行时间
    */1    *      *    *     *
    <分钟> <小时> <日> <月份> <星期>
 
    分钟 值从 0 到 59.
    小时 值从 0 到 23.
    日 值从 1 到 31.
    月 值从 1 到 12.
    星期 值从 0 到 6, 0 代表星期日
    多个时间可以用逗号隔开； 范围可以用连字符给出；*可以作为通配符； /表示每...
concurrencyPolicy:
    Allow:   允许Jobs并发运行(默认)
    Forbid:  禁止并发运行，如果上一次运行尚未完成，则跳过下一次运行
    Replace: 替换，取消当前正在运行的作业并用新作业替换它
```

```yaml
apiVersion: batch/v1beta1
kind: CronJob
metadata:
  name: pc-cronjob
  namespace: dev
  labels:
    controller: cronjob
spec:
  schedule: "*/1 * * * *"
  jobTemplate:
    metadata:
    spec:
      template:
        spec:
          restartPolicy: Never
          containers:
          - name: counter
            image: busybox:1.30
            command: ["bin/sh","-c","for i in 9 8 7 6 5 4 3 2 1; do echo $i;sleep 3;done"]
 
 
 
 
 
# kubectl create -f nginx-cronjob.yaml
# kubectl delete -f nginx-cronjob.yaml
# kubectl get cj -n dev
# kubectl get po -n dev
```

13、service
```yaml
kind: Service  # 资源类型
apiVersion: v1  # 资源版本
metadata: # 元数据
  name: service # 资源名称
  namespace: dev # 命名空间
spec: # 描述
  selector: # 标签选择器，用于确定当前service代理哪些pod
    app: nginx
  type: # Service类型，指定service的访问方式
  clusterIP:  # 虚拟服务的ip地址
  sessionAffinity: # session亲和性，支持ClientIP、None两个选项
  ports: # 端口信息
    - protocol: TCP 
      port: 3017  # service端口
      targetPort: 5003 # pod端口
      nodePort: 31122 # 主机端口
```

```
ClusterIP：默认值，它是Kubernetes系统自动分配的虚拟IP，只能在集群内部访问
NodePort：将Service通过指定的Node上的端口暴露给外部，通过此方法，就可以在集群外部访问服务
LoadBalancer：使用外接负载均衡器完成到服务的负载分发，注意此模式需要外部云环境支持
ExternalName： 把集群外部的服务引入集群内部，直接使用
```

```yaml
apiVersion: apps/v1
kind: Deployment      
metadata:
  name: pc-deployment
  namespace: dev
spec: 
  replicas: 3
  selector:
    matchLabels:
      app: nginx-pod
  template:
    metadata:
      labels:
        app: nginx-pod
    spec:
      containers:
      - name: nginx
        image: nginx:1.17.1
        ports:
        - containerPort: 80
```

```yaml
apiVersion: v1
kind: Service
metadata:
  name: service-clusterip
  namespace: dev
spec:
  selector:
    app: nginx-pod
  clusterIP: 10.97.97.97 # service的ip地址，如果不写，默认会生成一个
  type: ClusterIP
  ports:
  - port: 80  # Service端口       
    targetPort: 80 # pod端口
```

```bash
# kubectl create -f service-clusterip.yaml
# kubectl get svc -n dev -o wide
# kubectl describe svc service-clusterip -n dev
```

14、Endpoint
`# kubectl get ep  -n dev`

15、ingress
```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: ingress-https
  namespace: dev
spec:
  tls:
    - hosts:
      - nginx.itheima.com
      - tomcat.itheima.com
      secretName: tls-secret # 指定秘钥
  rules:
  - host: nginx.itheima.com
    http:
      paths:
      - path: /
        backend:
          serviceName: nginx-service
          servicePort: 80
  - host: tomcat.itheima.com
    http:
      paths:
      - path: /
        backend:
          serviceName: tomcat-service
          servicePort: 8080
```

```bash
# kubectl create -f xx.yaml
# kubectl get ing ingress-http -n dev
# kubectl describe ing ingress-http -n dev
```

16、数据存储
```
kubernetes的Volume支持多种类型，比较常见的有下面几个：

简单存储：EmptyDir、HostPath、NFS
高级存储：PV、PVC
配置存储：ConfigMap、Secret
```

EmptyDir：
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: volume-emptydir
  namespace: dev
spec:
  containers:
  - name: nginx
    image: nginx:1.17.1
    ports:
    - containerPort: 80
    volumeMounts:  # 将logs-volume挂在到nginx容器中，对应的目录为 /var/log/nginx
    - name: logs-volume
      mountPath: /var/log/nginx
  - name: busybox
    image: busybox:1.30
    command: ["/bin/sh","-c","tail -f /logs/access.log"] # 初始命令，动态读取指定文件中内容
    volumeMounts:  # 将logs-volume 挂在到busybox容器中，对应的目录为 /logs
    - name: logs-volume
      mountPath: /logs
  volumes: # 声明volume， name为logs-volume，类型为emptyDir
  - name: logs-volume
    emptyDir: {}
```

HostPath：
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: volume-hostpath
  namespace: dev
spec:
  containers:
  - name: nginx
    image: nginx:1.17.1
    ports:
    - containerPort: 80
    volumeMounts:
    - name: logs-volume
      mountPath: /var/log/nginx
  - name: busybox
    image: busybox:1.30
    command: ["/bin/sh","-c","tail -f /logs/access.log"]
    volumeMounts:
    - name: logs-volume
      mountPath: /logs
  volumes:
  - name: logs-volume
    hostPath: 
      path: /root/logs
      type: DirectoryOrCreate  # 目录存在就使用，不存在就先创建后使用
```

```
关于type的值的一点说明：

DirectoryOrCreate 目录存在就使用，不存在就先创建后使用

Directory 目录必须存在 FileOrCreate 文件存在就使用，不存在就先创建后使用

File 文件必须存在

Socket unix套接字必须存在

CharDevice 字符设备必须存在

BlockDevice 块设备必须存在
```

NFS：
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: volume-nfs
  namespace: dev
spec:
  containers:
  - name: nginx
    image: nginx:1.17.1
    ports:
    - containerPort: 80
    volumeMounts:
    - name: logs-volume
      mountPath: /var/log/nginx
  - name: busybox
    image: busybox:1.30
    command: ["/bin/sh","-c","tail -f /logs/access.log"] 
    volumeMounts:
    - name: logs-volume
      mountPath: /logs
  volumes:
  - name: logs-volume
    nfs:
      server: 192.168.5.6  #nfs服务器地址
      path: /root/data/nfs #共享文件路径
```

```
PV（Persistent Volume）是持久化卷的意思，是对底层的共享存储的一种抽象。一般情况下PV由kubernetes管理员进行创建和配置，它与底层具体的共享存储技术有关，并通过插件完成与共享存储的对接。

PVC（Persistent Volume Claim）是持久卷声明的意思，是用户对于存储需求的一种声明。换句话说，PVC其实就是用户向kubernetes系统发出的一种资源需求申请。
```

```yaml
apiVersion: v1  
kind: PersistentVolume
metadata:
  name: pv2
spec:
  nfs: # 存储类型，与底层真正存储对应
  capacity:  # 存储能力，目前只支持存储空间的设置
    storage: 2Gi
  accessModes:  # 访问模式
  storageClassName: # 存储类别
  persistentVolumeReclaimPolicy: # 回收策略
```

```
PV 的关键配置参数说明：

存储类型

底层实际存储的类型，kubernetes支持多种存储类型，每种存储类型的配置都有所差异

存储能力（capacity）

目前只支持存储空间的设置( storage=1Gi )，不过未来可能会加入IOPS、吞吐量等指标的配置

访问模式（accessModes）

用于描述用户应用对存储资源的访问权限，访问权限包括下面几种方式：

ReadWriteOnce（RWO）：读写权限，但是只能被单个节点挂载
ReadOnlyMany（ROX）： 只读权限，可以被多个节点挂载
ReadWriteMany（RWX）：读写权限，可以被多个节点挂载
需要注意的是，底层不同的存储类型可能支持的访问模式不同

回收策略（persistentVolumeReclaimPolicy）

当PV不再被使用了之后，对其的处理方式。目前支持三种策略：

Retain （保留） 保留数据，需要管理员手工清理数据
Recycle（回收） 清除 PV 中的数据，效果相当于执行 rm -rf /thevolume/*
Delete （删除） 与 PV 相连的后端存储完成 volume 的删除操作，当然这常见于云服务商的存储服务
需要注意的是，底层不同的存储类型可能支持的回收策略不同

存储类别

PV可以通过storageClassName参数指定一个存储类别

具有特定类别的PV只能与请求了该类别的PVC进行绑定
未设定类别的PV则只能与不请求任何类别的PVC进行绑定
状态（status）

一个 PV 的生命周期中，可能会处于4中不同的阶段：

Available（可用）： 表示可用状态，还未被任何 PVC 绑定
Bound（已绑定）： 表示 PV 已经被 PVC 绑定
Released（已释放）： 表示 PVC 被删除，但是资源还未被集群重新声明
Failed（失败）： 表示该 PV 的自动回收失败
```

PV： 
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name:  pv1
spec:
  capacity: 
    storage: 1Gi
  accessModes:
  - ReadWriteMany
  persistentVolumeReclaimPolicy: Retain
  nfs:
    path: /root/data/pv1
    server: 192.168.5.6
```

`# kubectl get pv -o wide`

PVC：
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc
  namespace: dev
spec:
  accessModes: # 访问模式
  selector: # 采用标签对PV选择
  storageClassName: # 存储类别
  resources: # 请求空间
    requests:
      storage: 5Gi
```

```
PVC 的关键配置参数说明：

访问模式（accessModes）
用于描述用户应用对存储资源的访问权限

选择条件（selector）

通过Label Selector的设置，可使PVC对于系统中己存在的PV进行筛选

存储类别（storageClassName）

PVC在定义时可以设定需要的后端存储的类别，只有设置了该class的pv才能被系统选出

资源请求（Resources ）

描述对存储资源的请求
```

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc1
  namespace: dev
spec:
  accessModes: 
  - ReadWriteMany
  resources:
    requests:
      storage: 1Gi
```

`# kubectl get pvc  -n dev -o wide`

使用pv：
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod1
  namespace: dev
spec:
  containers:
  - name: busybox
    image: busybox:1.30
    command: ["/bin/sh","-c","while true;do echo pod1 >> /root/out.txt; sleep 10; done;"]
    volumeMounts:
    - name: volume
      mountPath: /root/
  volumes:
    - name: volume
      persistentVolumeClaim:
        claimName: pvc1
        readOnly: false
```

```
PVC和PV是一一对应的，PV和PVC之间的相互作用遵循以下生命周期：

资源供应：管理员手动创建底层存储和PV

资源绑定：用户创建PVC，kubernetes负责根据PVC的声明去寻找PV，并绑定

在用户定义好PVC之后，系统将根据PVC对存储资源的请求在已存在的PV中选择一个满足条件的

一旦找到，就将该PV与用户定义的PVC进行绑定，用户的应用就可以使用这个PVC了
如果找不到，PVC则会无限期处于Pending状态，直到等到系统管理员创建了一个符合其要求的PV
PV一旦绑定到某个PVC上，就会被这个PVC独占，不能再与其他PVC进行绑定了

资源使用：用户可在pod中像volume一样使用pvc

Pod使用Volume的定义，将PVC挂载到容器内的某个路径进行使用。

资源释放：用户删除pvc来释放pv

当存储资源使用完毕后，用户可以删除PVC，与该PVC绑定的PV将会被标记为“已释放”，但还不能立刻与其他PVC进行绑定。通过之前PVC写入的数据可能还被留在存储设备上，只有在清除之后该PV才能再次使用。

资源回收：kubernetes根据pv设置的回收策略进行资源的回收

对于PV，管理员可以设定回收策略，用于设置与之绑定的PVC释放资源之后如何处理遗留数据的问题。只有PV的存储空间完成回收，才能供新的PVC绑定和使用
```

ConfigMap：
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: configmap
  namespace: dev
data:
  info: |
    username:admin
    password:123456
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-configmap
  namespace: dev
spec:
  containers:
  - name: nginx
    image: nginx:1.17.1
    volumeMounts: # 将configmap挂载到目录
    - name: config
      mountPath: /configmap/config
  volumes: # 引用configmap
  - name: config
    configMap:
      name: configmap
```

```bash
# kubectl create -f pod-configmap.yaml
# kubectl get pod pod-configmap -n dev
# kubectl exec -it pod-configmap -n dev /bin/sh
```

Secret：
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: secret
  namespace: dev
type: Opaque
data:
  username: YWRtaW4=
  password: MTIzNDU2
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-secret
  namespace: dev
spec:
  containers:
  - name: nginx
    image: nginx:1.17.1
    volumeMounts: # 将secret挂载到目录
    - name: config
      mountPath: /secret/config
  volumes:
  - name: config
    secret:
      secretName: secret
```

```bash
# kubectl create -f pod-secret.yaml
# kubectl get pod pod-secret -n dev
# kubectl exec -it pod-secret /bin/sh -n dev
```

17、安全认证
```
角色绑定、授权

Role、ClusterRole

RoleBinding、ClusterRoleBinding
```
