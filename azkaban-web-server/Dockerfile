FROM alpine

WORKDIR /data

ENV JAVA_HOME=/usr/lib/jvm/default-jvm \
    PATH=${PATH}:${JAVA_HOME}/bin 

# Azkaban web server port
EXPOSE 8443    

COPY azkaban-web-server.zip .

RUN set -eux \
    && echo 'https://mirrors.cloud.tencent.com/alpine/v3.11/main/' > /etc/apk/repositories \
    && echo 'https://mirrors.cloud.tencent.com/alpine/v3.11/community/' >> /etc/apk/repositories \
    && apk add --no-cache curl bash openjdk8-jre-base tzdata\
    && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo Asia/Shanghai > /etc/timezone \
    && apk del tzdata \
    && unzip azkaban-web-server.zip \
    && mkdir azkaban-web-server/logs \
    && rm -rf azkaban-web-server.zip /var/tmp/* /var/cache/apk/* 

# Define default workdir
WORKDIR azkaban-web-server

COPY conf/* conf/
COPY run.sh run.sh
RUN chmod +x run.sh

CMD ./run.sh && tail -f /dev/null
