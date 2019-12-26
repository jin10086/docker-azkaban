FROM openjdk:8-alpine

ENV AZK_VERSION 3.81.0 

WORKDIR /data

RUN set -eux \
    && apk add --update curl tzdata git \
    && curl -sLk https://github.com/azkaban/azkaban/archive/$AZK_VERSION.tar.gz| tar xzv \
    && cd azkaban-$AZK_VERSION \
    && ./gradlew build installDist -x test \
    && mv azkaban-web-server/build/install/azkaban-web-server ../ \
    && mv azkaban-db/build/install/azkaban-db/create-all-sql-0.1.0-SNAPSHOT.sql ../ \
    && mkdir ../azkaban-web-server/logs/ \
    && ln -snf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo Asia/Shanghai > /etc/timezone \
    && apk del tzdata curl git \
    && rm -rf azkaban-$AZK_VERSION /tmp/*  var/tmp/* /var/cache/apk/* ~/.gradle

COPY keystore azkaban-web-server/keystore
COPY conf/* azkaban-web-server/conf/

# Azkaban web server port
EXPOSE 8443

# Define default workdir
WORKDIR azkaban-web-server/
COPY script/run.sh run.sh
RUN chmod +x run.sh

#CMD ["bin/start-web.sh"]
CMD ["./run.sh"]
