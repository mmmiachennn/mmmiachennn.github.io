---
title: ClickHouse
tags:
  - OLAP
categories:
  - Develop
date: 2025-04-21
draft: true
---

## start docker
```
docker run -d --name ramjet_wo123 --env-file /root/aibdt/ramjet/.django.env registry.aibdt.com.tw/system/ramjet/ramjet:v5.2.0-development.7 celery -A ramjet worker -l info --pidfile=

docker run -d --name aaairflow -p 8080:8080 -v /home/mia/airflow/dags/:/usr/local/airflow/dags puckel/docker-airflow webserver

docker run -d --name ramjet123 --env-file /root/aibdt/ramjet/.django.env registry.aibdt.com.tw/system/ramjet/ramjet:v5.2.0-development.7
```

https://ithelp.ithome.com.tw/articles/10242460



## Docker Compose


### check log

```
docker-compose logs -f container_id
docker logs --tail 100 container_name
```

### shutdown container

```
docker-compose down
```

### start container backend

```
docker-compose up -d
```

### show images

```
docker images
```

### show container

```
docker ps -a
```


### run docker images bash

```
docker exec -it container_id bash
```
-i 即使沒有附加也保持 stdin 打開
-t 分配一個偽終端
example:
run docker images
docker run imageName echo 'hello world'


### check container logs

```
docker logs container_id
```


### pull images

```
docker pull registry_host/imageName:tag
# example:
# docker pull registry.hub.docker.com/ubuntu:latest
```


### check container id and container name

```
docker ps
```


### 複製檔案

```
docker cp /path/to/file1 DOCKER_ID:/path/to/file2
docker cp /path/to/folder DOCKER_ID:/another/path/


docker cp DOCKER_ID:/path/to/file1 /path/to/file2
docker cp DOCKER_ID:/path/to/folder /path/to/
```


### check image

```
docker images
```

### image detail

```
docker inspect image_id
```

### image param detail

docker inspect -f {{".Architecture"}} 550


### delete image

```
docker rmi imageName
docker rmi image_id
```

### about exsits image create new images
```
sudo docker run -it ubuntu:14.04 /bin/bash
touch test
exit
docker commit -m "Add a new file" -a "Mia" container_id new_imagesName
```

docker save
docker load



### 參考網站： https://www.runoob.com/docker/docker-command-manual.html

