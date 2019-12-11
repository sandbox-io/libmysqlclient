ARG ALPINE_VERSION=3.10

FROM alpine:$ALPINE_VERSION as builder

ARG MYSQL_VERSION=8.0.18

RUN cd /tmp \
  && apk --no-cache add build-base cmake ncurses-dev openssl-dev \
  && wget https://dev.mysql.com/get/Downloads/mysql-$MYSQL_VERSION.tar.gz -O - | tar zxf - \
  && mkdir -p mysql-$MYSQL_VERSION/build \
  && cd mysql-$MYSQL_VERSION/build \
  && cmake .. -DDOWNLOAD_BOOST=1 -DWITH_BOOST=/tmp -DWITHOUT_SERVER=1 \
  && make && make install

FROM alpine:$ALPINE_VERSION
RUN apk --no-cache add ncurses-libs libgcc libstdc++
COPY --from=builder /usr/local/mysql /usr/local/mysql
