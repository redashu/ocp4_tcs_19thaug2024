# ocp4_tcs_19thaug2024

### Understanding openshift Engineering 

<img src="oce.png">

### Overall ocp user and security 

<img src="usersec.png">

### cleaning up resources in ocp 

```
oc whoami
kube:admin
[ashu@ip-172-31-16-156 ~]$ oc  get  project | grep -i ashu
ashu-app-project                                                  Active
ashu-poc-web1                                                     Active
[ashu@ip-172-31-16-156 ~]$ oc  get all -n ashu-app-project 
Warning: apps.openshift.io/v1 DeploymentConfig is deprecated in v4.14+, unavailable in v4.10000+
NAME                          READY   STATUS    RESTARTS   AGE
pod/ashuapp-fbcd8f4bb-r5wz9   1/1     Running   3          2d20h

NAME                      READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/ashuapp   1/1     1            1           2d20h

NAME                                DESIRED   CURRENT   READY   AGE
replicaset.apps/ashuapp-fbcd8f4bb   1         1         1       2d20h
[ashu@ip-172-31-16-156 ~]$ oc delete all --all -n ashu-app-project 
pod "ashuapp-fbcd8f4bb-r5wz9" deleted
deployment.apps "ashuapp" deleted
replicaset.apps "ashuapp-fbcd8f4bb" deleted
Warning: apps.openshift.io/v1 DeploymentConfig is deprecated in v4.14+, unavailable in v4.10000+
[ashu@ip-172-31-16-156 ~]$ oc delete all --all -n ashu-poc-web1 
Warning: apps.openshift.io/v1 DeploymentConfig is deprecated in v4.14+, unavailable in v4.10000+
No resources found

```

### useful commands 

```
oc  whoami  --show-server 
https://api.tcs-cluster.ashutoshh.xyz:6443

[ashu@ip-172-31-16-156 ~]$ oc  whoami  --show-console 
https://console-openshift-console.apps.tcs-cluster.ashutoshh.xyz
[ashu@ip-172-31-16-156 ~]$ 

```
### enable tab completion in opeshift cli 

```
vim   ~/.bashrc 

# add below in the very last
source <(oc completion bash)
```

### save file and load it 

```
source  ~/.bashrc
```

### now you can use tab with oc cli 

```
oc whoami --show-
--show-console  (If true, print the current server's web console URL)
--show-context  (Print the current user context name)
--show-server   (If true, print the current server's REST API URL)
--show-token    (Print the token the current session is using. This will return an error if you are using a different form of authentication.)
[ashu@ip-172-31-16-156 ~]$ oc whoami --show-

```

### dashboard url 

```
 oc whoami --show-console 
https://console-openshift-console.apps.tcs-cluster.ashutoshh.xyz

```


