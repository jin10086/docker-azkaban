FROM alpine

WORKDIR /data

# Azkaban web server port
EXPOSE 8443

COPY azkaban-web-server.zip .

RUN set -eux \
    && apk add --update bash openjdk8-jre-base tzdata\
    && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo Asia/Shanghai > /etc/timezone \
    && apk del tzdata \
    && unzip azkaban-web-server.zip \
    && mkdir azkaban-web-server/logs \
    && rm -rf azkaban-web-server.zip /var/tmp/* /var/cache/apk/* 

ENV AZKABAN_OPTS "-Xmx512m" \
	JAVA_HOME /usr/lib/jvm/default-jvm \
	PATH ${PATH}:${JAVA_HOME}/bin

# Define default workdir
WORKDIR azkaban-web-server

COPY keystore keystore
COPY conf/* conf/
COPY run.sh run.sh
RUN chmod +x run.sh

CMD ./run.sh && tail -f /dev/null
