### testing last build image by creating container 

## From docker image to container 

```
docker  run --name ashuwebc1  -d  ashuwebapp:appv1 
0aeb76d52e9379f31410819d33db81accc1234eb0a98f3b5082ab78e437c5047


[ashu@ip-172-31-16-156 ~]$ docker  ps
CONTAINER ID   IMAGE              COMMAND                 CREATED          STATUS          PORTS     NAMES
0aeb76d52e93   ashuwebapp:appv1   "httpd -D FOREGROUND"   4 seconds ago    Up 3 seconds              ashuwebc1

```

### checking resources of containers 

```
[ashu@ip-172-31-16-156 ~]$ docker  stats  ashuwebc1

CONTAINER ID   NAME        CPU %     MEM USAGE / LIMIT     MEM %     NET I/O     BLOCK I/O     PIDS
0aeb76d52e93   ashuwebc1   0.09%     17.16MiB / 7.744GiB   0.22%     936B / 0B   0B / 8.19kB   213


```

### port mapping in container networking 

<img src="portm.png">

