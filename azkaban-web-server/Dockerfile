FROM openjdk:8-alpine

ENV AZK_VERSION 3.81.0 

WORKDIR /root

RUN apk add --update curl tzdata \
    && curl -sLk https://github.com/azkaban/azkaban/archive/$AZK_VERSION.tar.gz| tar xz \
    && cd azkaban-$AZK_VERSION \
    && ./gradlew build \
    && ./gradlew installDist \
    && mv azkaban-web-server/build/install/azkaban-web-server ../ \
    && cd .. \
    && mkdir azkaban-web-server/logs/ \
    && sed -i "s/&//" azkaban-web-server/bin/start-web.sh \
    && apk del tzdata  curl \
    && rm -rf /tmp/* /var/cache/apk/*

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
