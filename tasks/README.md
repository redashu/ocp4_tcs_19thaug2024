# openshift tassk 

## task 1 for dealing with pods 

```
  1. Create pod named  <yourname>pod1
  2. In POd docker image will be busybox 
  3. choose ping fb.com as default process
  4. check output of default process and store in a file  called logs.txt in client machine 
  5. now transfer logs.txt on your pods under /opt/logs.txt 
  6. check that pods is scheduled in which minion node and store that name in logs.txt inside pod 
  7. make sure previous data in logs.txt must be present 
  

```

### task 2 

```
  1. Create a yaml file named  <yourname>splunk.yaml
  
  3. use splunk/splunk:latest image from docker hub  to create deployment   
  5. Required password must be stored in Secret 
  6. replica count 1 
  7. create service of ClusterIP  type named <yourname>svc  and routes as well 
  8. access this from web browser 
  9. Note : default username of splunk is admin and splunk default port number is 8000 
  
```