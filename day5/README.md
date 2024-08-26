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