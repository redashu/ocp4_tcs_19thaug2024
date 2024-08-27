# ocp4_tcs_19thaug2024

### openshift cluster Understanding 

<img src="oc.png">

### understanding user and project relations 

<img src="user1.png">

### users type in oc platform 

<img src="user2.png">


### loging to super admin user using oc cli 

```
oc  login  https://api.tcs-cluster.ashutoshh.xyz:6443  -u kubeadmin -p "your kubeadmin password"  
error: tls: failed to verify certificate: x509: certificate signed by unknown authority

=====>>> to solve this error 
[ashu@ip-172-31-16-156 ~]$ oc  login  https://api.tcs-cluster.ashutoshh.xyz:6443  -u kubeadmin -p "your kubeadmin password"  --insecure-skip-tls-verify
WARNING: Using insecure TLS client config. Setting this option is not supported!

Login successful.

You have access to 76 projects, the list has been suppressed. You can list all projects with 'oc projects'

Using project "ashu-app-project".
[ashu@ip-172-31-16-156 ~]$ 
[ashu@ip-172-31-16-156 ~]$ oc  whoami
kube:admin
[ashu@ip-172-31-16-156 ~]$ 

```