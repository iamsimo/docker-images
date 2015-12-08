FROM ubuntu

MAINTAINER Simone Sarcoli <simone.sarcoli@gmail.com>

RUN apt-get update
RUN apt-get -y install xmlstarlet bsdtar unzip curl
RUN apt-get -y install software-properties-common python-software-properties

RUN groupadd -r wildfly -g 1000 && useradd -u 1000 -r -g wildfly -m -d /opt/wildfly -s /sbin/nologin -c "Wildfly service user" wildfly

WORKDIR /opt/wildfly

USER root

RUN add-apt-repository ppa:webupd8team/java
RUN apt-get update
RUN echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | sudo /usr/bin/debconf-set-selections
RUN apt-get -y install oracle-java8-installer
RUN apt-get -y install oracle-java8-set-default

USER wildfly

ENV WILDFLY_VERSION 9.0.2.Final

RUN cd $HOME && curl -O http://download.jboss.org/wildfly/$WILDFLY_VERSION/wildfly-$WILDFLY_VERSION.zip && unzip wildfly-$WILDFLY_VERSION.zip && mv $HOME/wildfly-$WILDFLY_VERSION $HOME/wildfly && rm wildfly-$WILDFLY_VERSION.zip

ENV JBOSS_HOME /opt/wildfly/wildfly

EXPOSE 8080 9990

CMD ["/opt/wildfly/wildfly/bin/standalone.sh", "-c", "standalone.xml", "-b", "0.0.0.0","-bmanagement", "0.0.0.0"]
