FROM python:3.7.6-alpine3.10

WORKDIR /data

ENV LANG=C.UTF-8 \
	JAVA_HOME=/usr/lib/jvm/default-jvm \
    PATH=${PATH}:${JAVA_HOME}/bin 

# Azkaban executor port
EXPOSE 12321

COPY azkaban-exec-server.zip .

# coreutils https://www.cnblogs.com/shansongxian/p/10531439.html
RUN set -eux \
    && echo 'https://mirrors.cloud.tencent.com/alpine/v3.11/main/' > /etc/apk/repositories \
    && echo 'https://mirrors.cloud.tencent.com/alpine/v3.11/community/' >> /etc/apk/repositories \
	&& apk add --no-cache bash coreutils mysql-client mongodb-tools postgresql-client openjdk8-jre-base tzdata \
    && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo Asia/Shanghai > /etc/timezone \
    && apk del tzdata \
    && unzip azkaban-exec-server.zip \
    && rm -rf azkaban-exec-server.zip /var/tmp/* /var/cache/apk/* 

# Define default workdir
WORKDIR azkaban-exec-server

COPY conf/* conf/
COPY run.sh run.sh
COPY pip.conf /root/.pip/pip.conf
RUN chmod +x run.sh

CMD ./run.sh && tail -f /dev/null