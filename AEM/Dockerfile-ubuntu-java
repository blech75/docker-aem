# Java base image (used by aem-base)

FROM ubuntu:14.04

ENV DEBIAN_FRONTEND noninteractive
ENV JAVA_VERSION 7
ENV JAVA_HOME /usr/lib/jvm/java-7-oracle

# Install updates and Java 7
RUN apt-get update && \
    apt-get install -y curl unzip software-properties-common && \
    add-apt-repository ppa:webupd8team/java && \
    apt-get update && \
    apt-get -y upgrade && \
    echo oracle-java${JAVA_VERSION}-installer shared/accepted-oracle-license-v1-1 select true | debconf-set-selections && \
    apt-get -y install oracle-java7-installer && \
    apt-get clean && \
    apt-get purge && \
    update-alternatives --display java

# Cleanup installation
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    rm -rf /var/cache/oracle-jdk7-installer
