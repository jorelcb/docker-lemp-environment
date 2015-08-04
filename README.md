# Docker LEMP Environment

### Docker Containers for a complete Lemp Environment

>As Super User do.

    docker run --name app-data -v /home/myuser/workspace:/srv/www:rw -v /home/myuser/dbdata:/srv/dbdata:rw jorelcb/app-data && \
    docker run --name mysql -e MYSQL_DATABASE=myappdb -e MYSQL_ROOT_PASSWORD=1234 --volumes-from app-data -d jorelcb/mysql && \
    docker run --privileged=true --name phpfpm --volumes-from app-data --link mysql:mysql -d jorelcb/phpfpm && \
    docker run --privileged=true --name nginx --volumes-from app-data -p 80:80 --link phpfpm:fpm -d jorelcb/nginx


In order to add app database to your mysql docker container yo need to execute: 

>(first add myapp.sql to /srv/dbdata folder)

    docker run -it --volumes-from app-data --link mysql:mysql --rm jorelcb/mysql sh -c 'exec mysql -h"$MYSQL_PORT_3306_TCP_ADDR" -P"$MYSQL_PORT_3306_TCP_PORT" -uroot -p"$MYSQL_ENV_MYSQL_ROOT_PASSWORD" "MYSQL_DATABASE" < /srv/dbdata/myapp.sql'