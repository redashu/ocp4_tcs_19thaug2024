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

