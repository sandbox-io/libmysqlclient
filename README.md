## libmysqlclient

### Why I wrote this Docker image

Alpine package for mysql (mysql-dev) installs libmysqlclient as a symlink to libmariadb.

```sh
docker run -it --rm ruby:alpine sh

apk --no-cache add mysql-dev

ls -la /usr/lib/libmysqlclient.so
lrwxrwxrwx    1 root     root            15 May  8 03:05 /usr/lib/libmysqlclient.so -> libmariadb.so.3
```

We've got strange SEGFAULT with libmariadb when connecting to AWS RDS reader endpoint. I found the error didn't occur with libmysqlclient.
https://github.com/brianmario/mysql2/issues/715
https://github.com/brianmario/mysql2/issues/915

So I needed libmysqlclient itself instead of libmariadb to connect to RDS reader.


### Example usage

When installing mysql2 gem on Alpine ruby image.

```Dockerfile
FROM ruby:alpine

RUN apk --no-cache add \
  build-base \
  libstdc++ \
  openssl-dev

# use libmysqlclient instead of libmariadb to build mysql2 gem
COPY --from=sndbx/libmysqlclient /usr/local/mysql /usr/local/mysql

RUN gem install mysql2
```
