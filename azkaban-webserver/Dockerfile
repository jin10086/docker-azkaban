# VERSION 1.0
# AUTHOR: Matthieu "Puckel_" Roisil
# DOCKER HUB: https://hub.docker.com/u/puckel/
# DESCRIPTION: Simple Debian image for azkaban-webserver based on official java:8
# BUILD: docker build --rm -t puckel/azkaban-webserver .
# SOURCE: https://github.com/puckel/docker-azkaban

FROM java:8

# Never prompts the user for choices on installation/configuration of packages
ENV DEBIAN_FRONTEND noninteractive
ENV TERM linux
# Work around initramfs-tools running on kernel 'upgrade': <http://bugs.debian.org/cgi-bin/bugreport.cgi?bug=594189>
ENV INITRD No

ENV AZK_VERSION 3.43.0
ENV MYSQL_JDBC_VERSION 5.1.35

WORKDIR /root

RUN apt-get update -yqq \
    && apt-get install -yqq \
    curl \
    netcat \
    mysql-client \
    && curl -sLk https://github.com/azkaban/azkaban/archive/$AZK_VERSION.tar.gz| tar xz \
    && cd azkaban-$AZK_VERSION \
    && ./gradlew build \
    && ./gradlew installDist \
    && mkdir azkaban-web-server/build/install/azkaban-web-server/logs/ \
    && sed -i "s/&//" azkaban-web-server/build/install/azkaban-web-server/bin/azkaban-web-start.sh \
    && apt-get clean \
    && rm -rf \
    /var/lib/apt/lists/* \
    /tmp/* \
    /var/tmp/* \
    /usr/share/man \
    /usr/share/doc \
    /usr/share/doc-base

COPY keystore azkaban-$AZK_VERSION/azkaban-web-server/build/install/azkaban-web-server/keystore
COPY conf/* azkaban-$AZK_VERSION/azkaban-web-server/build/install/azkaban-web-server/conf/

# Azkaban web server port
EXPOSE 8443

# Define default workdir
WORKDIR /root/azkaban-$AZK_VERSION/azkaban-web-server/build/install/azkaban-web-server/
COPY script/run.sh run.sh
RUN chmod +x run.sh

CMD ["./run.sh"]
