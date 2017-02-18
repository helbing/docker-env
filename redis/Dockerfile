FROM redis:latest
MAINTAINER helbing <helbingxxx@gmail.com>

VOLUME ["/data"]

COPY redis.conf /usr/local/etc/redis/redis.conf
CMD ["redis-server", "/usr/local/etc/redis/redis.conf"]

EXPOSE 6379
