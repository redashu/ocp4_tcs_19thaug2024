
### checking lab details 

```

[ashu@ip-172-31-16-156 ~]$ oc whoami
test1
[ashu@ip-172-31-16-156 ~]$ oc  project
Using project "ashu-personal" on server "https://api.tcs-cluster.ashutoshh.xyz:6443".
[ashu@ip-172-31-16-156 ~]$ oc  get all
Warning: apps.openshift.io/v1 DeploymentConfig is deprecated in v4.14+, unavailable in v4.10000+
NAME                           READY   STATUS    RESTARTS   AGE
pod/ashu-ui-745c8cc5c8-rqg47   1/1     Running   1          21h
pod/ashudb-587c998967-f8qh9    1/1     Running   1          20h

NAME              TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
service/ashdblb   ClusterIP   172.30.116.106   <none>        3306/TCP   20h
service/weblb     ClusterIP   172.30.134.43    <none>        8080/TCP   21h

NAME                      READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/ashu-ui   1/1     1            1           21h
deployment.apps/ashudb    1/1     1            1           20h

NAME                                 DESIRED   CURRENT   READY   AGE
replicaset.apps/ashu-ui-745c8cc5c8   1         1         1       21h
replicaset.apps/ashudb-587c998967    1         1         1       20h

NAME                             HOST/PORT                                            PATH   SERVICES   PORT   TERMINATION   WILDCARD
route.route.openshift.io/weblb   weblb-ashu-personal.apps.tcs-cluster.ashutoshh.xyz          weblb      8080                 None
[ashu@ip-172-31-16-156 ~]$ oc  delete all --all
pod "ashu-ui-745c8cc5c8-rqg47" deleted
pod "ashudb-587c998967-f8qh9" deleted
service "ashdblb" deleted
service "weblb" deleted
deployment.apps "ashu-ui" deleted
deployment.apps "ashudb" deleted
Warning: apps.openshift.io/v1 DeploymentConfig is deprecated in v4.14+, unavailable in v4.10000+
route.route.openshift.io "weblb" deleted

```

### CoreDNS understanding 

<img src="coredns1.png">

### task updates 

### creating webapp in ashu-personal namespace 

```
[ashu@ip-172-31-16-156 tasks]$ oc  create  -f  webdeploy.yaml   -f websvc.yml 
deployment.apps/ashu-ui created
service/weblb created

[ashu@ip-172-31-16-156 tasks]$ oc  get  deploy
NAME      READY   UP-TO-DATE   AVAILABLE   AGE
ashu-ui   1/1     1            1           4s

[ashu@ip-172-31-16-156 tasks]$ oc  get  svc
NAME    TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
weblb   ClusterIP   172.30.88.151   <none>        8080/TCP   8s

[ashu@ip-172-31-16-156 tasks]$ oc  get  ep
NAME    ENDPOINTS          AGE
weblb   10.130.2.19:8080   11s

[ashu@ip-172-31-16-156 tasks]$ oc  expose service weblb 
route/weblb exposed
[ashu@ip-172-31-16-156 tasks]$ oc  get  routes
NAME    HOST/PORT                                            PATH   SERVICES   PORT   TERMINATION   WILDCARD
weblb   weblb-ashu-personal.apps.tcs-cluster.ashutoshh.xyz          weblb      8080                 None
[ashu@ip-172-31-16-156 tasks]$ 

```

### creating a new-project and there deployment of db 

```
[ashu@ip-172-31-16-156 tasks]$ oc  new-project  ashu-db-space 
Now using project "ashu-db-space" on server "https://api.tcs-cluster.ashutoshh.xyz:6443".

You can add applications to this project with the 'new-app' command. For example, try:

    oc new-app rails-postgresql-example

to build a new example application in Ruby. Or use kubectl to deploy a simple Kubernetes application:

    kubectl create deployment hello-node --image=registry.k8s.io/e2e-test-images/agnhost:2.43 -- /agnhost serve-hostname

[ashu@ip-172-31-16-156 tasks]$ oc  project
Using project "ashu-db-space" on server "https://api.tcs-cluster.ashutoshh.xyz:6443".
[ashu@ip-172-31-16-156 tasks]$ ls
db_deloy.yaml  dbsvc.yml  rootsecret.yml  webdeploy.yaml  websvc.yml
[ashu@ip-172-31-16-156 tasks]$ oc  create  -f rootsecret.yml -f db_deloy.yaml  -f dbsvc.yml 
secret/ashu-db-creds created
deployment.apps/ashudb created
service/ashdblb created
[ashu@ip-172-31-16-156 tasks]$ oc  get  secret
NAME                       TYPE                      DATA   AGE
ashu-db-creds              Opaque                    1      6s
builder-dockercfg-psg2d    kubernetes.io/dockercfg   1      51s
default-dockercfg-kgdc7    kubernetes.io/dockercfg   1      51s
deployer-dockercfg-gp225   kubernetes.io/dockercfg   1      51s
[ashu@ip-172-31-16-156 tasks]$ oc  get  deploy
NAME     READY   UP-TO-DATE   AVAILABLE   AGE
ashudb   1/1     1            1           10s
[ashu@ip-172-31-16-156 tasks]$ oc  get  po
NAME                      READY   STATUS    RESTARTS   AGE
ashudb-587c998967-8msjs   1/1     Running   0          14s
[ashu@ip-172-31-16-156 tasks]$ oc  get  svc
NAME      TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
ashdblb   ClusterIP   172.30.62.232   <none>        3306/TCP   19s

```

### checking from pod1 to pod2 

```
ashu@ip-172-31-16-156 ~]$ oc get deploy 
NAME     READY   UP-TO-DATE   AVAILABLE   AGE
ashudb   1/1     1            1           27m
[ashu@ip-172-31-16-156 ~]$ oc  run  webclient --image=alpine  --command sleep 100000 
pod/webclient created
[ashu@ip-172-31-16-156 ~]$ oc  get  pods
NAME                      READY   STATUS    RESTARTS   AGE
ashudb-587c998967-8msjs   1/1     Running   0          27m
webclient                 1/1     Running   0          4s
[ashu@ip-172-31-16-156 ~]$ 
[ashu@ip-172-31-16-156 ~]$ oc get project  | grep -i ashu
ashu-db-space                  Active
ashu-personal                  Active
[ashu@ip-172-31-16-156 ~]$ oc  get  pods -o wide  -n  ashu-personal 
NAME                       READY   STATUS    RESTARTS   AGE   IP            NODE                          NOMINATED NODE   READINESS GATES
ashu-ui-745c8cc5c8-h9jnp   1/1     Running   0          34m   10.130.2.19   ip-10-0-90-155.ec2.internal   <none>           <none>
[ashu@ip-172-31-16-156 ~]$ 
[ashu@ip-172-31-16-156 ~]$ 
[ashu@ip-172-31-16-156 ~]$ oc  get pods
NAME                      READY   STATUS    RESTARTS   AGE
ashudb-587c998967-8msjs   1/1     Running   0          30m
webclient                 1/1     Running   0          2m25s
[ashu@ip-172-31-16-156 ~]$ oc  exec -it webclient -- sh 
~ $ 
~ $ ping  10.130.2.19
PING 10.130.2.19 (10.130.2.19): 56 data bytes
64 bytes from 10.130.2.19: seq=0 ttl=42 time=1.755 ms
64 bytes from 10.130.2.19: seq=1 ttl=42 time=0.801 ms
^C
--- 10.130.2.19 ping statistics ---

```

### overall cross project networking in openshfit service

<img src="svcn1.png">

## RBAC in ocp 

### roles and role bindings

<img src="role1.png">


### creating a project using admin user 

```
[ashu@ip-172-31-16-156 ~]$ oc whoami 
kube:admin
[ashu@ip-172-31-16-156 ~]$ 
[ashu@ip-172-31-16-156 ~]$ oc  new-project   ashu-poc-web
Now using project "ashu-poc-web" on server "https://api.tcs-cluster.ashutoshh.xyz:6443".

You can add applications to this project with the 'new-app' command. For example, try:

    oc new-app rails-postgresql-example

to build a new example application in Ruby. Or use kubectl to deploy a simple Kubernetes application:

    kubectl create deployment hello-node --image=registry.k8s.io/e2e-test-images/agnhost:2.43 -- /agnhost serve-hostname

[ashu@ip-172-31-16-156 ~]$ 



```

### login with personal user and try to access above project 

```
 oc  login  https://api.tcs-cluster.ashutoshh.xyz:6443  -u test1  -p Ocp@12345  --insecure-skip-tls-verify
WARNING: Using insecure TLS client config. Setting this option is not supported!

Login successful.

You have one project on this server: "ashu-personal"

Using project "ashu-personal".
[ashu@ip-172-31-16-156 users_manifest]$ oc project ashu-poc-web
error: You are not a member of project "ashu-poc-web".
You have one project on this server: ashu-personal
[ashu@ip-172-31-16-156 users_manifest]$ 

```

### creating role under ashu-poc-web project 

```
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  creationTimestamp: null
  name: pod-access
  namespace: ashu-poc-web
rules:
- apiGroups: [""]
  resources: ["pods","services"]
  verbs: ["*"]

```

### creating it 

```
 oc  create -f role1.yaml 
role.rbac.authorization.k8s.io/pod-access created
[ashu@ip-172-31-16-156 rbac]$ oc  get roles -n ashu-poc-web 
NAME         CREATED AT
pod-access   2024-08-28T11:18:31Z

```


### verify details 

```
 oc  project  ashu-poc-web 
Now using project "ashu-poc-web" on server "https://api.tcs-cluster.ashutoshh.xyz:6443".
[ashu@ip-172-31-16-156 rbac]$ 
[ashu@ip-172-31-16-156 rbac]$ oc  get roles 
NAME         CREATED AT
pod-access   2024-08-28T11:18:31Z
[ashu@ip-172-31-16-156 rbac]$ oc describe roles pod-access 
Name:         pod-access
Labels:       <none>
Annotations:  <none>
PolicyRule:
  Resources  Non-Resource URLs  Resource Names  Verbs
  ---------  -----------------  --------------  -----
  pods       []                 []              [*]
  services   []                 []              [*]

```

### creating rolebinding which will be bounded to user 

```
 oc  create rolebinding  ashurolebind1 --role pod-access  --user=test1  --dry-run=client -o yaml 
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  creationTimestamp: null
  name: ashurolebind1
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: pod-access
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: User
  name: test1
[ashu@ip-172-31-16-156 rbac]$ oc  create rolebinding  ashurolebind1 --role pod-access  --user=test1  --dry-run=client -o yaml  >rolebind1.yaml 
[ashu@ip-172-31-16-156 rbac]$ oc create -f rolebind1.yaml 
rolebinding.rbac.authorization.k8s.io/ashurolebind1 created
[ashu@ip-172-31-16-156 rbac]$ oc get rolebinding
NAME                    ROLE                               AGE
admin                   ClusterRole/admin                  62m
ashurolebind1           Role/pod-access                    8s
system:deployers        ClusterRole/system:deployer        62m
system:image-builders   ClusterRole/system:image-builder   62m
system:image-pullers    ClusterRole/system:image-puller    62m

```

### final step by step guide 

<img src=""step1.png">

## verify 

```
oc  login  https://api.tcs-cluster.ashutoshh.xyz:6443  -u test1  -p Ocp@12345  --insecure-skip-tls-verify
WARNING: Using insecure TLS client config. Setting this option is not supported!

Login successful.

You have one project on this server: "ashu-personal"

Using project "ashu-personal".
[ashu@ip-172-31-16-156 rbac]$ 
[ashu@ip-172-31-16-156 rbac]$ 
[ashu@ip-172-31-16-156 rbac]$ oc  whoami
test1
[ashu@ip-172-31-16-156 rbac]$ oc  get pods -n  ashu-poc-web 
No resources found in ashu-poc-web namespace.
[ashu@ip-172-31-16-156 rbac]$ 
[ashu@ip-172-31-16-156 rbac]$ oc  get service  -n  ashu-poc-web 
No resources found in ashu-poc-web namespace.
[ashu@ip-172-31-16-156 rbac]$ oc  get deploy  -n  ashu-poc-web 
Error from server (Forbidden): deployments.apps is forbidden: User "test1" cannot list resource "deployments" in API group "apps" in the namespace "ashu-poc-web"
[ashu@ip-172-31-16-156 rbac]$ 
[ashu@ip-172-31-16-156 rbac]$ oc  get routes  -n  ashu-poc-web 
Error from server (Forbidden): routes.route.openshift.io is forbidden: User "test1" cannot list resource "routes" in API group "route.openshift.io" in the namespace "ashu-poc-web"

```

### in OCP type of roles 

<img src="rtype.png">

### pre define role in ocp 

[click_here](https://docs.openshift.com/container-platform/4.8/authentication/using-rbac.html)

### creating a new project for testing 

```
oc whoami
test1
[ashu@ip-172-31-16-156 ~]$ oc logout 
Logged "test1" out on "https://api.tcs-cluster.ashutoshh.xyz:6443"
[ashu@ip-172-31-16-156 ~]$ oc login   https://api.tcs-cluster.ashutoshh.xyz:6443  -u kubeadmin -p pkSo3-puJpN-uNugw-XBPjC  --insecure-skip-tls-verify
WARNING: Using insecure TLS client config. Setting this option is not supported!

Login successful.

You have access to 91 projects, the list has been suppressed. You can list all projects with 'oc projects'

Using project "ashu-personal".
[ashu@ip-172-31-16-156 ~]$ 
[ashu@ip-172-31-16-156 ~]$ oc delete project  ashu-poc-web 
project.project.openshift.io "ashu-poc-web" deleted
[ashu@ip-172-31-16-156 ~]$ 
[ashu@ip-172-31-16-156 ~]$ oc  new-project    ashu-poc-web` 
> ^C
[ashu@ip-172-31-16-156 ~]$ oc  new-project    ashu-poc-web1
Now using project "ashu-poc-web1" on server "https://api.tcs-cluster.ashutoshh.xyz:6443".

You can add applications to this project with the 'new-app' command. For example, try:

    oc new-app rails-postgresql-example

to build a new example application in Ruby. Or use kubectl to deploy a simple Kubernetes application:

    kubectl create deployment hello-node --image=registry.k8s.io/e2e-test-images/agnhost:2.43 -- /agnhost serve-hostname

[ashu@ip-172-31-16-156 ~]$ 

```

### using oc policy command to bind predefine roles to user 

```
oc  whoami
kube:admin
[ashu@ip-172-31-16-156 ~]$ oc  policy add-role-to-user  view     test1 
clusterrole.rbac.authorization.k8s.io/view added: "test1"
[ashu@ip-172-31-16-156 ~]$ 
[ashu@ip-172-31-16-156 ~]$ 
[ashu@ip-172-31-16-156 ~]$ oc  login  https://api.tcs-cluster.ashutoshh.xyz:6443  -u test1  -p Ocp@12345  --insecure-skip-tls-verify
WARNING: Using insecure TLS client config. Setting this option is not supported!

Login successful.

You have access to the following projects and can switch between them with 'oc project <projectname>':

    ashu-personal
  * ashu-poc-web1

Using project "ashu-poc-web1".
[ashu@ip-172-31-16-156 ~]$ oc  get  pods 
No resources found in ashu-poc-web1 namespace.
[ashu@ip-172-31-16-156 ~]$ oc  get  pods -n ashu-poc-web1
No resources found in ashu-poc-web1 namespace.
[ashu@ip-172-31-16-156 ~]$ oc  create  deployment d1 --image=nginx 
error: failed to create deployment: deployments.apps is forbidden: User "test1" cannot create resource "deployments" in API group "apps" in the namespace "ashu-poc-web1"
[ashu@ip-172-31-16-156 ~]$ 

```

### Roles 

<img src="roles11.png">

### clusterRole and clusterRole bindings 

<img src="clsr.png">





