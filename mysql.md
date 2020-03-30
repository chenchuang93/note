# docker下创建容器
docker run -p 3306:3306 --name mysql -e MYSQL_ROOT_PASSWORD=123456 -d mysql

docker run --name mysql -p3306:3306 -e MYSQL_ROOT_PASSWORD=123qweasd -d mysql:5.7.27

172.31.131.90