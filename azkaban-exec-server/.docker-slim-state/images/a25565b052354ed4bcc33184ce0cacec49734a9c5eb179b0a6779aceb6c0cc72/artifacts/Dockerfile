FROM scratch
LABEL docker-slim.version="darwin|Transformer|1.26.1|2ec04e169b12a87c5286aa09ef44eac1cea2c7a1|2019-11-28_04:37:59PM"
ADD files.tar /
WORKDIR /data/azkaban-exec-server
ENV PATH "/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/lib/jvm/java-1.8-openjdk/jre/bin:/usr/lib/jvm/java-1.8-openjdk/bin"
ENV LANG "C.UTF-8"
ENV JAVA_HOME "/usr/lib/jvm/java-1.8-openjdk"
ENV JAVA_VERSION "8u212"
ENV JAVA_ALPINE_VERSION "8.212.04-r0"
EXPOSE 12321/tcp
CMD ["/bin/sh"]
