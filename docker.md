# docker安装
ubuntu下 
```
sudo apt-get update
sudo apt-get install docker.io
```

sudo apt-get remove docker-engine docker.io  
sudo apt-get remove docker-ce  
sudo apt-get autoremove docker
# 启动docker
service docker start  
# 查看容器列表
sudo docker ps -a 
# 启动容器
sudo docker start container-name/container-id
# 新建容器
docker run -d -p 9090:8080 -p 1521:1521 --name xxxx wnameless/oracle-xe-11g  
# 登陆容器
docker exec -it container-id/container-name bash

docker exec -it container-id/container-name redis-cli
# 帮助文档
docker --help