# VERSION 1.0
# AUTHOR: Matthieu "Puckel_" Roisil
# DOCKER HUB: https://hub.docker.com/u/puckel/
# DESCRIPTION: Simple Debian image for azkaban-executor based on official java:8
# BUILD: docker build --rm -t puckel/azkaban-executor .
# SOURCE: https://github.com/puckel/docker-azkaban

FROM java:8

# Never prompts the user for choices on installation/configuration of packages
ENV DEBIAN_FRONTEND noninteractive
ENV TERM linux
# Work around initramfs-tools running on kernel 'upgrade': <http://bugs.debian.org/cgi-bin/bugreport.cgi?bug=594189>
ENV INITRD No

ENV AZK_VERSION 3.25.1

WORKDIR /root

RUN apt-get update -yqq \
    && apt-get install -yqq \
    curl \
    && curl -sLk https://github.com/azkaban/azkaban/archive/$AZK_VERSION.tar.gz| tar xz \
    && cd azkaban-$AZK_VERSION \
    && ./gradlew build \
    && ./gradlew installDist \
    && sed -i "s/&//" azkaban-exec-server/build/install/azkaban-exec-server/bin/azkaban-executor-start.sh \
    && apt-get clean \
    && rm -rf \
    /var/lib/apt/lists/* \
    /tmp/* \
    /var/tmp/* \
    /usr/share/man \
    /usr/share/doc \
    /usr/share/doc-base

COPY conf/* azkaban-$AZK_VERSION/azkaban-exec-server/build/install/azkaban-exec-server/conf/
COPY plugins/jobtypes/commonprivate.properties azkaban-$AZK_VERSION/azkaban-exec-server/build/install/azkaban-exec-server/plugins/jobtypes/commonprivate.properties

# Azkaban executor port
EXPOSE 12321

# Define default workdir
WORKDIR /root/azkaban-$AZK_VERSION/azkaban-exec-server/build/install/azkaban-exec-server/

CMD ["bin/azkaban-executor-start.sh"]
