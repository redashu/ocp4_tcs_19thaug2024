### openshift lab infra

<img src="infra.png">

## Understanding api resources of apiServer in openshift platform 

<img src="res.png">

### delete resources forcefull 

```
oc delete pods --all --force
Warning: Immediate deletion does not wait for confirmation that the running resource has been terminated. The resource may continue to run on the cluster indefinitely.
pod "akki" force deleted
pod "ashupod1" force deleted
pod "asifpod1" force deleted
pod "macpod" force deleted
pod "ranjitpod" force deleted
pod "sachingpod" force deleted
pod "sachinpod11" force deleted
pod "sidpod1" force deleted

```

## understanding problem with pods 

<img src="pods1.png">

### openshift controllers

<img src="occ.png">

## understanding RC->RS and downtime 

<img src="ocs.png">

### FINALLy pods using deployment controller 

<img src="podsc.png">

### Deployment controller in final terms 

<img src="dd.png">


## deploying container image using deployment 

### creating YAML file using oc cli 

```
227  oc  run  ashupod2 --image=dockerashu/ashutcs:webappv1 --port 80  --dry-run=client 
  228  oc  get pods
  229  oc  run  ashupod2 --image=dockerashu/ashutcs:webappv1 --port 80  --dry-run=client  -o yaml 
  230  oc  run  ashupod2 --image=dockerashu/ashutcs:webappv1 --port 80  --dry-run=client  -o yaml  >autopod.yaml 
  231  oc  run  ashupod2 --image=dockerashu/ashutcs:webappv1 --port 80  --dry-run=client  -o json 
  232  oc  run  ashupod2 --image=dockerashu/ashutcs:webappv1 --port 80  --dry-run=client  -o json >autopod.json

```

### creating pods 

```
[ashu@ip-172-31-16-156 ocp_manifests]$ oc  create -f autopod.json 
pod/ashupod2 created
[ashu@ip-172-31-16-156 ocp_manifests]$ 
[ashu@ip-172-31-16-156 ocp_manifests]$ 
[ashu@ip-172-31-16-156 ocp_manifests]$ 
[ashu@ip-172-31-16-156 ocp_manifests]$ 
[ashu@ip-172-31-16-156 ocp_manifests]$ 
[ashu@ip-172-31-16-156 ocp_manifests]$ oc  get  pods
NAME         READY   STATUS    RESTARTS   AGE
ashupod2     1/1     Running   0          5s
sachinpod2   1/1     Running   0          90s

```

### we can delete using file based request

```
oc  delete -f  autopod.json 
pod "ashupod2" deleted

```

### Deleting all the pods 

```
oc delete pods --all
pod "akki2" deleted
pod "akkijson" deleted
pod "asifpod2" deleted
pod "mypod" deleted
pod "sachindpod4" deleted
pod "sachindpod5" deleted
pod "sachinpod2" deleted
pod "sidpod2" deleted
pod "sidpod3" deleted

```

### creating deployment controller manifest yaml file 

```
oc  create deployment  ashuapp --image=dockerashu/ashutcs:webappv1  --port 80  --dry-run=client -o yaml >deploy1.yaml
```

### deploying controller

```
[ashu@ip-172-31-16-156 ocp_manifests]$ oc  create  -f deploy1.yaml 
deployment.apps/ashuapp created

===> checking 
[ashu@ip-172-31-16-156 ocp_manifests]$ oc  get  deployment 
NAME      READY   UP-TO-DATE   AVAILABLE   AGE
ashuapp   1/1     1            1           13s

===> Deployment -- create RS (replicaset)

ashu@ip-172-31-16-156 ocp_manifests]$ oc  get  replicasets
NAME               DESIRED   CURRENT   READY   AGE
ashuapp-99bdc766   1         1         1       81s
[ashu@ip-172-31-16-156 ocp_manifests]$ 

===> Replicasets -create pods 

 oc  get  pods
NAME                     READY   STATUS    RESTARTS   AGE
ashuapp-99bdc766-4tr5p   1/1     Running   0          87s
```




