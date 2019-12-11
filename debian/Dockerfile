ARG DEBIAN_VERSION=buster

FROM debian:$DEBIAN_VERSION-slim as builder

ARG MYSQL_VERSION=8.0.18

RUN cd /tmp \
  && apt-get update && apt-get install -y --no-install-recommends \
  build-essential ca-certificates cmake curl libncurses-dev libssl-dev \
  && curl -L https://dev.mysql.com/get/Downloads/mysql-$MYSQL_VERSION.tar.gz | tar zxf - \
  && mkdir -p mysql-$MYSQL_VERSION/build \
  && cd mysql-$MYSQL_VERSION/build \
  && cmake .. -DDOWNLOAD_BOOST=1 -DWITH_BOOST=/tmp -DWITHOUT_SERVER=1 \
  && make && make install

FROM debian:$DEBIAN_VERSION-slim
RUN apt-get update && apt-get install -y --no-install-recommends libncurses6 libssl1.1 \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*

COPY --from=builder /usr/local/mysql /usr/local/mysql
