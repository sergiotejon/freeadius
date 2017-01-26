# Description

FreerRADIUS Server.

It depends on a MySQL and Redis Server to work and allows to configure those server connections via environment variables. See down below. 

# Running the container

$ docker run -d -t freeradius -p 1812/udp:1812/sdp -p 1813/udp:1813/udp -e DB_HOST=mysql.server -e REDIS_HOST=redis.server stejon/freeradius

# Environment Variables

- DB_HOST=localhost
- DB_PORT=3306
- DB_USER=radius
- DB_PASS=radpass
- DB_NAME=radius
- RADIUS_KEY=testing123
- REDIS_HOST=localhost
- REDIS_PORT=6379

# Docker Compose Example

Next an example of a docker-compose.yml file:

```
version: '2'
services:
   freeradius:
      image: stejon/freeradius
      ports:
         - 1812/udp:1812/udp
         - 1813/udp:1813/udp
      environment:
           #- DB_NAME=radius
         - DB_HOST=db
           #- DB_USER=radius
           #- DB_PASS=radpass
           #- DB_PORT=3306
           #- RADIUS_KEY=testing123
         - REDIS_HOST=redis
           #- REDIS_PORT=6379
      depends_on:
         - db
         - redis
      links:
         - db
         - redis
      restart: always

   db:
     image: mysql
     ports:
        - "3306:3306"
     volumes:
        - ./data:/var/lib/mysql
        - ./radius.sql:/docker-entrypoint-initdb.d/radius.sql
        - ./conf.d/:/etc/mysql/conf.d/
     environment:
      MYSQL_ROOT_PASSWORD: radius
      MYSQL_USER: radius
      MYSQL_PASSWORD: radpass
      MYSQL_DATABASE: radius
     restart: always

   redis:
      image: redis
      restart: always
```

Note: This example binds freeradius with a mysql database. Take note of conf.d dir volume, as it contains specific configuration from mysql:

File: conf.d/max_allowed_packer.cnf
```
max_allowed_packet=256M
```

An SQL scheme for FreeRadius on MySQL can be found here: https://wiki.freeradius.org/config/MySQL-DDL-script
