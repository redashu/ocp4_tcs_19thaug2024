# ocp4_tcs_19thaug2024

### OPenshift overall 2 sides of understanding 

<img src="oci.png">

### openshift app deployment required check list 

<img src="req.png">

### cleaning up project resources 

```
[ashu@ip-172-31-16-156 ~]$ oc  whoami 
kube:admin
===>>
oc  get projects  | grep -i ashu
ashu-app-project                                                  Active
ashu-poc-web1                                                     Active
[ashu@ip-172-31-16-156 ~]$ oc  delete all --all -n ashu-app-project 
Warning: apps.openshift.io/v1 DeploymentConfig is deprecated in v4.14+, unavailable in v4.10000+
No resources found
[ashu@ip-172-31-16-156 ~]$ oc  delete all --all -n ashu-poc-web1 
Warning: apps.openshift.io/v1 DeploymentConfig is deprecated in v4.14+, unavailable in v4.10000+
No resources found

```

### login with non root user and creating a project 

```
oc  new-project   ashu-ocp-final 
Now using project "ashu-ocp-final" on server "https://api.tcs-cluster.ashutoshh.xyz:6443".

You can add applications to this project with the 'new-app' command. For example, try:

    oc new-app rails-postgresql-example

to build a new example application in Ruby. Or use kubectl to deploy a simple Kubernetes application:

    kubectl create deployment hello-node --image=registry.k8s.io/e2e-test-images/agnhost:2.43 -- /agnhost serve-hostname

[ashu@ip-172-31-16-156 ~]$ 
[ashu@ip-172-31-16-156 ~]$ oc project
Using project "ashu-ocp-final" on server "https://api.tcs-cluster.ashutoshh.xyz:6443".
[ashu@ip-172-31-16-156 ~]$ ls
html-sample-app  kubeconfig  ocp_manifests
[ashu@ip-172-31-16-156 ~]$ cd  ocp_manifests/
[ashu@ip-172-31-16-156 ocp_manifests]$ ls
ashuwebpod.yaml  autopod.json  autopod.yaml  day6_deploy.yaml  day6route.yaml  day6svc.yml  deploy1.yaml  private.yaml  regsecret.yaml  service.yaml  storage  users_manifest
[ashu@ip-172-31-16-156 ocp_manifests]$ mkdir  finalapp 
[ashu@ip-172-31-16-156 ocp_manifests]$ cd finalapp/
[ashu@ip-172-31-16-156 finalapp]$ ls
[ashu@ip-172-31-16-156 finalapp]$ 

```

