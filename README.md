# Dockerfile-Redis


#Redis Dockerfile

#https://github.com/dockerfile/redis
#Pull base image.
FROM dockerfile/ubuntu

#Install Redis & conf.
RUN apt-get update && \
    apt-get install build-essential tcl && \
    apt-get install redis-server && \
  cd /tmp && \
  apt-get install curl && \
  curl -O http://download.redis.io/redis-stable.tar.gz && \
  tar xzvf redis-stable.tar.gz && \
  cd redis-stable && \
  make && \
  make test  && \
  make install && \
sed -i "s/^supervised no/supervised systemd/" /etc/redis/redis.conf && \
sed -i "s/^dir \.\//dir \/var\/lib\/redis/" /etc/redis/redis.conf && \
sed -i "s@^# requirepass foobared@requirepass mortezaie1373@"  /etc/redis/redis.conf && \
service redis-server restart
#Define mountable directories.
VOLUME ["/data"]

#Define working directory.
WORKDIR /data

#Expose ports.
EXPOSE 6379

#Define default command.
CMD ["redis-server", "/etc/redis/redis.conf"]
