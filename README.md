# Description

FreerRADIUS Server.

It depends on a MySQL and Redis Server to work and allows to configure those server connections via environment variables. See down below. 

# Running the container

$ docker run -d -t freeradius -p 1812/udp:1812/sdp -p 1813/udp:1813/udp -e DB_HOST=mysql.server -e REDIS_HOST=redis.server stejon/freeradius

# Environment Variables

DB_HOST=localhost
DB_PORT=3306
DB_USER=radius
DB_PASS=radpass
DB_NAME=radius
RADIUS_KEY=testing123
REDIS_HOST=localhost
REDIS_PORT=6379
