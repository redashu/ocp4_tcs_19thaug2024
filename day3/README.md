# ocp4_tcs_19thaug2024

## k8s revision 

<img src="rev.png">

### checking existing container image 

```
[ashu@ip-172-31-16-156 ~]$ who
sachinG  pts/0        2024-08-22 08:38 (103.221.250.71)
vivek    pts/1        2024-08-22 08:40 (171.48.84.72)
sachinD  pts/2        2024-08-22 08:45 (152.58.28.240)
ec2-user pts/3        2024-08-22 08:50 (106.215.50.153)
ec2-user pts/4        2024-08-22 08:50 (106.215.50.153)
[ashu@ip-172-31-16-156 ~]$ 
[ashu@ip-172-31-16-156 ~]$ docker  images  | grep ashu
dockerashu/ashutcs             webappv1   dcaa1198974f   2 days ago     538MB
ashuwebapp                     appv1      dcaa1198974f   2 days ago     538MB
[ashu@ip-172-31-16-156 ~]$ 


```

### Understanding lab infra 

<img src="lab.png">

### checking kubectl {k8s client side software}


```
kubectl  version --client 
Client Version: v1.29.7
Kustomize Version: v5.0.4-0.20230601165947-6ce0bf390ce3

```

### if you want to install kubectl software you can install using link 

[click_here](https://kubernetes.io/docs/tasks/tools/)


### k8s workflow understanding 

<img src="workf.png">



### downloading kubeconfig file to users home directory 

```
 cp -v  /tmp/kubeconfig   . 
'/tmp/kubeconfig' -> './kubeconfig'
[ashu@ip-172-31-16-156 ~]$ ls
html-sample-app  kubeconfig
[ashu@ip-172-31-16-156 ~]$ 

```

### checking nodes details 

```
[ashu@ip-172-31-16-156 ~]$ ls
html-sample-app  kubeconfig
[ashu@ip-172-31-16-156 ~]$ 
[ashu@ip-172-31-16-156 ~]$ kubectl    get   nodes    --kubeconfig  kubeconfig 
NAME                          STATUS   ROLES                  AGE     VERSION
ip-10-0-21-91.ec2.internal    Ready    control-plane,master   4h59m   v1.29.6+aba1e8d
ip-10-0-31-38.ec2.internal    Ready    control-plane,master   4h59m   v1.29.6+aba1e8d
ip-10-0-51-187.ec2.internal   Ready    control-plane,master   4h59m   v1.29.6+aba1e8d
ip-10-0-80-83.ec2.internal    Ready    worker                 4h42m   v1.29.6+aba1e8d
ip-10-0-90-155.ec2.internal   Ready    worker                 4h41m   v1.29.6+aba1e8d
ip-10-0-95-205.ec2.internal   Ready    worker                 4h42m   v1.29.6+aba1e8d
ip-10-0-95-84.ec2.internal    Ready    worker                 4h42m   v1.29.6+aba1e8d
[ashu@ip-172-31-16-156 ~]$ 


```

### checking more details 

```
kubectl    version     --kubeconfig  kubeconfig 
Client Version: v1.29.7
Kustomize Version: v5.0.4-0.20230601165947-6ce0bf390ce3
Server Version: v1.29.6+aba1e8d
[ashu@ip-172-31-16-156 ~]$ kubectl    custer-info    --kubeconfig  kubeconfig 
error: unknown command "custer-info" for "kubectl"

Did you mean this?
	cluster-info
[ashu@ip-172-31-16-156 ~]$ kubectl    cluster-info    --kubeconfig  kubeconfig 
Kubernetes control plane is running at https://api.tcs-cluster.ashutoshh.xyz:6443

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
[ashu@ip-172-31-16-156 ~]$ 

```

## Default file and place from where kubectl command can read creds 

<img src="cred.png">


### Demo 

```
[ashu@ip-172-31-16-156 ~]$ mkdir  ~/.kube 
mkdir: cannot create directory ‘/home/ashu/.kube’: File exists
[ashu@ip-172-31-16-156 ~]$ 
[ashu@ip-172-31-16-156 ~]$ ls
html-sample-app  kubeconfig
[ashu@ip-172-31-16-156 ~]$ 
[ashu@ip-172-31-16-156 ~]$ cp kubeconfig   ~/.kube/config  
[ashu@ip-172-31-16-156 ~]$ 
[ashu@ip-172-31-16-156 ~]$ ls  ~/.kube/
cache  config

```

### making the same change 

```
[ashu@ip-172-31-16-156 ~]$ kubectl   get nodes
NAME                          STATUS   ROLES                  AGE     VERSION
ip-10-0-21-91.ec2.internal    Ready    control-plane,master   5h7m    v1.29.6+aba1e8d
ip-10-0-31-38.ec2.internal    Ready    control-plane,master   5h7m    v1.29.6+aba1e8d
ip-10-0-51-187.ec2.internal   Ready    control-plane,master   5h7m    v1.29.6+aba1e8d
ip-10-0-80-83.ec2.internal    Ready    worker                 4h50m   v1.29.6+aba1e8d
ip-10-0-90-155.ec2.internal   Ready    worker                 4h50m   v1.29.6+aba1e8d
ip-10-0-95-205.ec2.internal   Ready    worker                 4h50m   v1.29.6+aba1e8d
ip-10-0-95-84.ec2.internal    Ready    worker                 4h50m   v1.29.6+aba1e8d
[ashu@ip-172-31-16-156 ~]$ kubectl  version 
Client Version: v1.29.7
Kustomize Version: v5.0.4-0.20230601165947-6ce0bf390ce3
Server Version: v1.29.6+aba1e8d
[ashu@ip-172-31-16-156 ~]$ kubectl  cluster-info 
Kubernetes control plane is running at https://api.tcs-cluster.ashutoshh.xyz:6443


```

