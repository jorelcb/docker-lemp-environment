# docker-lemp-environment
Docker Containers for a complete Lemp Environment

As Super User do.

docker run --name app-data -v /home/myuser/Workspace:/srv/www:rw jorelcb/app-data && \
docker run --name mysql -e MYSQL_ROOT_PASSWORD=1234 -d jorelcb/mysql && \
docker run --privileged=true --name phpfpm --volumes-from app-data --link mysql:mysql -d jorelcb/phpfpm && \
docker run --privileged=true --name nginx --volumes-from app-data -p 80:80 --link phpfpm:fpm -d jorelcb/nginx

