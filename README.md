# Dockerfile-Redis

#https://github.com/dockerfile/redis
FROM ubuntu:16.04

#Install Redis.
RUN   apt-get update \
 && apt-get install -y build-essential tcl \
 && apt-get install -y redis-server \


#Download and Extract the Source Code
  &&  cd /tmp \
  &&  apt-get install curl  \
  &&  curl -O http://download.redis.io/redis-stable.tar.gz  \
  &&  tar xzvf redis-stable.tar.gz  \
   &&  cd redis-stable \
   
#Build and Install Redis
  &&  make  \
  &&  make test \
  &&  make install  \
    
#Configure Redis
  &&  sed -i "s/^supervised no/supervised systemd/" /etc/redis/redis.conf  \
  &&  sed -i "s/^dir \.\//dir \/var\/lib\/redis/" /etc/redis/redis.conf  \
  &&  sed -i "s@^# requirepass foobared@requirepass mortezaie1373@"  /etc/redis/redis.conf  \
    service redis-server restart
#Define mountable directories.
  VOLUME ["/data"]

#Define working directory.
WORKDIR /data

#Expose ports.
EXPOSE 6379

#Define default command.
CMD ["redis-server", "/etc/redis/redis.conf"]
