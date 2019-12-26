FROM alpine

WORKDIR /data

# Azkaban executor port
EXPOSE 12321

COPY azkaban-exec-server.zip .

RUN set -eux \
	&& apk add bash mysql-client mongodb-tools postgresql-client openjdk8-jre-base tzdata \
    && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo Asia/Shanghai > /etc/timezone \
    && apk del tzdata \
    && unzip azkaban-exec-server.zip \
    && mkdir azkaban-exec-server/logs \
    && rm -rf azkaban-exec-server.zip /var/tmp/* /var/cache/apk/* 

ENV AZKABAN_OPTS "-Xmx512m" \
	JAVA_HOME /usr/lib/jvm/default-jvm \
	PATH ${PATH}:${JAVA_HOME}/bin

# Define default workdir
WORKDIR azkaban-exec-server

COPY conf/* conf/
COPY run.sh run.sh
RUN chmod +x run.sh

CMD ./run.sh && tail -f /dev/null