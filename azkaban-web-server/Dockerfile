FROM openjdk:8-alpine

WORKDIR /data

# Azkaban web server port
EXPOSE 8443

COPY azkaban-web-server.zip .

RUN set -eux \
    && apk add --update tzdata\
    && ln -snf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo Asia/Shanghai > /etc/timezone \
    && apk del tzdata \
    && rm -rf azkaban-web-server.zip /tmp/* var/tmp/* /var/cache/apk/* 

COPY keystore azkaban-web-server/keystore
COPY conf/* azkaban-web-server/conf/

# Define default workdir
WORKDIR azkaban-web-server/
COPY script/run.sh run.sh
RUN chmod +x run.sh

#CMD ["bin/start-web.sh"]
CMD ["./run.sh"]
