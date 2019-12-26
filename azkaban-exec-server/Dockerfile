FROM openjdk:8-alpine

WORKDIR /data

# Azkaban executor port
EXPOSE 12321

COPY azkaban-exec-server.zip .

RUN set -eux \
	&& apk add --update mysql-client mongodb-tools postgresql-client tzdata \
    && ln -snf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo Asia/Shanghai > /etc/timezone \
    && apk del tzdata \
    && unzip azkaban-exec-server.zip \
    && rm -rf azkaban-exec-server.zip /tmp/* var/tmp/* /var/cache/apk/* 

COPY conf/* azkaban-exec-server/conf/

# Define default workdir
WORKDIR azkaban-exec-server/

CMD ["bin/start-exec.sh"]
