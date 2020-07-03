FROM redis
RUN mkdir /var/log/redis
COPY ./redis.conf /usr/local/etc/redis/redis.conf
CMD [ "redis-server", "/usr/local/etc/redis/redis.conf" ]