官方容器的使用
mysql
文档:https://hub.docker.com/_/mysql/

启动:
docker run -d \
--name mysql \
-h servername \
--restart always \
-e MYSQL_ROOT_PASSWORD=123 \
-v /etc/localtime:/etc/localtime:ro \
-v /opt/docker/data/mysql/etc:/etc/mysql/ \
-v /opt/docker/data/mysql/data:/var/lib/mysql \
-p 3306:3306 \
mysql:5.7.23


查看默认配置
docker run -it --rm mysql:tag --verbose --help

操作:
docker exec -it test sh -c "mysql -p"

使用mysqldump
$ docker exec some-mysql sh -c 'exec mysqldump --all-databases -uroot -p"$MYSQL_ROOT_PASSWORD"' > /some/path/on/your/host/all-databases.sql



rabbitmq
启动:
docker run -d \
--hostname my-rabbit \
--name some-rabbit \
--restart always \
-p 15672:15672 \
-p 5672:5672 \
-v /etc/localtime:/etc/localtime \
-v /opt/docker/data/rabbitmq/data:/var/lib/rabbitmq \
-v /opt/docker/data/rabbitmq/etc:/etc/rabbitmq \
rabbitmq:3-management


redis
启动:
$ docker run -d \
--name some-redis \
--restart always \
-v /etc/localtime:/etc/localtime \
-v /opt/docker/data/redis/data:/data \
-p 6379:6379 \
redis redis-server --appendonly yes

redis-cli使用
$ docker run -it --link some-redis:redis --rm redis redis-cli -h redis -p 6379

自定义Dockerfile
FROM redis
CMD [ "redis-server", "/usr/local/etc/redis/redis.conf" ]



mongodb

$ docker run -d \
--restart always \
--name some-mongo \
-v /etc/localtime:/etc/localtime \
-v /opt/docker/data/mongo/etc:/etc/mongo \
-v /opt/docker/data/mongo/data/:/data/db \
-p 27017:27017 \
mongo:3.4 --config /etc/mongo/mongod.conf 


docker run -it --link some-mongo:mongo --rm mongo:3.4 mongo --host mongo test


nginx

docker run -d \
--name nginx \
--restart always \
-v /etc/localtime:/etc/localtime \
-v /opt/docker/data/nginx/conf:/etc/nginx/conf.d:ro \
-v /opt/docker/data/nginx/data:/usr/share/nginx/html \
-v /opt/docker/data/nginx/log:/var/log/nginx \
-p 80:80 \
-p 443:443 \
nginx

apahce官方镜像

docker run -d \
--name apache \
--restart always \
-v /data/htdocs:/usr/local/apache2/htdocs \
-v /data/etc/apache/:/usr/local/apache2/conf/ \
-v /etc/localtime:/etc/localtime:ro \
-p 80:80 \
httpd:2.4
