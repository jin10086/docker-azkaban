FROM openjdk:8-alpine

ENV AZK_VERSION 3.81.0

WORKDIR /root

RUN set -eux \
	&& apk add --update curl tzdata \
    && curl -sLk https://github.com/azkaban/azkaban/archive/$AZK_VERSION.tar.gz| tar xz \
    && cd azkaban-$AZK_VERSION \
    && ./gradlew build \
    && ./gradlew installDist \
    && mv azkaban-exec-server/build/install/azkaban-exec-server ../ \
    && cd .. \
    && mkdir azkaban-exec-server/logs/ \
    $$ sed -i "s/&//" azkaban-exec-server/bin/start-exec.sh \
    && ln -snf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo Asia/Shanghai > /etc/timezone \
    && apk del tzdata curl \
    && rm -rf azkaban-$AZK_VERSION /tmp/* /var/cache/apk/* 

COPY conf/* azkaban-exec-server/conf/
COPY plugins/jobtypes/commonprivate.properties azkaban-exec-server/plugins/jobtypes/commonprivate.properties

# Azkaban executor port
EXPOSE 12321

# Define default workdir
WORKDIR azkaban-exec-server/

CMD ["bin/start-exec.sh"]
