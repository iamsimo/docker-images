FROM ubuntu

MAINTAINER Simone Sarcoli <simone.sarcoli@gmail.com>

#Update repos and install all the required packages
RUN add-apt-repository ppa:webupd8team/java
RUN apt-get update
RUN apt-get -y install xmlstarlet bsdtar unzip curl
RUN apt-get -y install software-properties-common python-software-properties

#Add wildfly service user
RUN groupadd -r wildfly -g 1000 && useradd -u 1000 -r -g wildfly -m -d /opt/wildfly -s /sbin/nologin -c "Wildfly service user" wildfly

#Setting working directory to wildfly home dir
WORKDIR /opt/wildfly

#Switching to root to install Oracle JDK 8
USER root

RUN echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | sudo /usr/bin/debconf-set-selections
RUN apt-get -y install oracle-java8-installer
RUN apt-get -y install oracle-java8-set-default

#Switching to wildfly to install Wildfly AS
USER wildfly
#Set Wildfly version
ENV WILDFLY_VERSION 9.0.2.Final
#Install Wildfly
RUN cd $HOME && curl -O http://download.jboss.org/wildfly/$WILDFLY_VERSION/wildfly-$WILDFLY_VERSION.zip && unzip wildfly-$WILDFLY_VERSION.zip && mv $HOME/wildfly-$WILDFLY_VERSION $HOME && rm wildfly-$WILDFLY_VERSION.zip
#Set JBOSS_HOME
ENV JBOSS_HOME /opt/wildfly

EXPOSE 8080 9990

#Add wildfly admin management user
RUN /opt/wildfly/bin/add-user.sh admin admin --silent

#Start Wildfly
CMD ["/opt/wildfly/bin/standalone.sh", "-c", "standalone.xml", "-b", "0.0.0.0","-bmanagement", "0.0.0.0"]
