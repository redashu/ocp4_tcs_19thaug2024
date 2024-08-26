### Revision 

## checking projects 

```
 oc whoami
  432  oc  get nodes
  433  oc  project
  434  oc get  project
  435  oc project  ashu-app-project
  436  history 
  437  oc  project

```

## checking current project 

```
 oc  project 
Using project "ashu-app-project" on server "https://api.tcs-cluster.ashutoshh.xyz:6443".
```

### In openshift 3 onwards 

- if we deploy any docker image which is having root user access then it is not allowed in OCP project

```
[ashu@ip-172-31-16-156 ocp_manifests]$ docker  run -dit  --name ashucx1  dockerashu/ashutcs:webappv1 
9f1355cfef3194b578de728d96abe2be188772017a4572e9c17ab63a2fa9062b
[ashu@ip-172-31-16-156 ocp_manifests]$ docker ps
CONTAINER ID   IMAGE                         COMMAND                 CREATED         STATUS         PORTS     NAMES
9f1355cfef31   dockerashu/ashutcs:webappv1   "httpd -D FOREGROUND"   4 seconds ago   Up 2 seconds             ashucx1
[ashu@ip-172-31-16-156 ocp_manifests]$ 
[ashu@ip-172-31-16-156 ocp_manifests]$ 
[ashu@ip-172-31-16-156 ocp_manifests]$ docker  exec -it ashucx1  bash 
[root@9f1355cfef31 /]# 
[root@9f1355cfef31 /]# whoami
root

```

### we need to allow any container userid to run in openshift project 

```
oc adm policy add-scc-to-user anyuid -z default
# to allow current project to use any container image with any username 
```

### Creating deployment and checking it 

```
 oc  create  deployment  ashuday6app --image=dockerashu/ashutcs:webappv1 --port 80 --dry-run=client -o yaml >day6_deploy.yaml 
[ashu@ip-172-31-16-156 ocp_manifests]$ oc create  -f day6_deploy.yaml 
deployment.apps/ashuday6app created
[ashu@ip-172-31-16-156 ocp_manifests]$ oc  get  deploy 
NAME          READY   UP-TO-DATE   AVAILABLE   AGE
ashuday6app   1/1     1            1           4s
[ashu@ip-172-31-16-156 ocp_manifests]$ oc  get  pods
NAME                          READY   STATUS    RESTARTS   AGE
ashuday6app-f6ccbc8c4-mtr6l   1/1     Running   0          9s
[ashu@ip-172-31-16-156 ocp_manifests]$ 


```

### doing a manual scaling 

```
 oc  scale deployment  ashuday6app  --replicas 2 
deployment.apps/ashuday6app scaled
[ashu@ip-172-31-16-156 ocp_manifests]$ oc  get  deploy 
NAME          READY   UP-TO-DATE   AVAILABLE   AGE
ashuday6app   2/2     2            2           4m35s
[ashu@ip-172-31-16-156 ocp_manifests]$ oc  get  pods
NAME                          READY   STATUS    RESTARTS   AGE
ashuday6app-f6ccbc8c4-gb5mq   1/1     Running   0          6s
ashuday6app-f6ccbc8c4-mtr6l   1/1     Running   0          4m37s

```

### creating clusterIP type service by exposing deployment 

```
oc  get deploy 
NAME          READY   UP-TO-DATE   AVAILABLE   AGE
ashuday6app   2/2     2            2           6m16s
[ashu@ip-172-31-16-156 ocp_manifests]$ 
[ashu@ip-172-31-16-156 ocp_manifests]$ oc expose deployment  ashuday6app   --type ClusterIP --port 80 --name ashulb6 --dry-run=client -o yaml >day6svc.yml
[ashu@ip-172-31-16-156 ocp_manifests]$ 
[ashu@ip-172-31-16-156 ocp_manifests]$ oc get svc 
No resources found in ashu-app-project namespace.
[ashu@ip-172-31-16-156 ocp_manifests]$ oc get service 
No resources found in ashu-app-project namespace.
[ashu@ip-172-31-16-156 ocp_manifests]$ oc create -f day6svc.yml 
service/ashulb6 created
[ashu@ip-172-31-16-156 ocp_manifests]$ oc get  svc
NAME      TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)   AGE
ashulb6   ClusterIP   172.30.3.32   <none>        80/TCP    3s
[ashu@ip-172-31-16-156 ocp_manifests]$ oc get  ep 
NAME      ENDPOINTS                       AGE
ashulb6   10.130.2.43:80,10.131.0.16:80   16s
[ashu@ip-172-31-16-156 ocp_manifests]$ oc  get po -o wide
NAME                          READY   STATUS    RESTARTS   AGE     IP            NODE                          NOMINATED NODE   READINESS GATES
ashuday6app-f6ccbc8c4-gb5mq   1/1     Running   0          3m      10.130.2.43   ip-10-0-90-155.ec2.internal   <none>           <none>
ashuday6app-f6ccbc8c4-mtr6l   1/1     Running   0          7m31s   10.131.0.16   ip-10-0-95-84.ec2.internal    <none>           <none>
[ashu@ip-172-31-16-156 ocp_manifests]$ 



```

### Understanding how end users are going to reach to app running in pods 

<img src="net1.png">

### Ingress controller to filter network traffic and forward connection to service of project

<img src="project1.png">

### Ingress controller in visual 

<img src="ingress.png">

### ocp setup + ingress + external LB 

<img src="setup.png">

### verify ingress controller in OCP 

```
oc  get projects  | grep ingress
openshift-ingress                                                 Active
openshift-ingress-canary                                          Active
openshift-ingress-operator                                        Active
[ashu@ip-172-31-16-156 ocp_manifests]$ 
[ashu@ip-172-31-16-156 ocp_manifests]$ 
[ashu@ip-172-31-16-156 ocp_manifests]$ 
[ashu@ip-172-31-16-156 ocp_manifests]$ 
[ashu@ip-172-31-16-156 ocp_manifests]$ oc  get  deploy  -n  openshift-ingress
NAME             READY   UP-TO-DATE   AVAILABLE   AGE
router-default   2/2     2            2           4d6h
[ashu@ip-172-31-16-156 ocp_manifests]$ 
[ashu@ip-172-31-16-156 ocp_manifests]$ oc  get  pod  -n  openshift-ingress
NAME                              READY   STATUS    RESTARTS        AGE
router-default-77fdf7dd84-g97xh   1/1     Running   4               4d6h
router-default-77fdf7dd84-r6bq8   1/1     Running   5 (4h55m ago)   4d6h
[ashu@ip-172-31-16-156 ocp_manifests]$ 
[ashu@ip-172-31-16-156 ocp_manifests]$ oc  get svc  -n  openshift-ingress
NAME                      TYPE           CLUSTER-IP       EXTERNAL-IP                                                               PORT(S)                      AGE
router-default            LoadBalancer   172.30.37.144    a7f5853abac8446e0a4e3f884856458a-1366741115.us-east-1.elb.amazonaws.com   80:32674/TCP,443:31734/TCP   4d6h
router-internal-default   ClusterIP      172.30.146.218   <none>                                                                    80/TCP,443/TCP,1936/TCP      4d6h

```

### Internal DNS you have to entry for route domain 

<img src="route1.png">

### openshift on prim responsibility 

<img src="route2.png">

