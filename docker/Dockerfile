FROM ubuntu:latest

RUN apt update
RUN apt-get update
RUN apt install wget -y
WORKDIR /opt

RUN wget --no-cookies --no-check-certificate \
    --header "Cookie: oraclelicense=accept-securebackup-cookie" \
    "https://download.oracle.com/java/23/latest/jdk-23_linux-x64_bin.tar.gz" \
    -O jdk-23_linux-x64_bin.tar.gz

RUN tar -xzvf /opt/jdk-23_linux-x64_bin.tar.gz -C /opt && \
    rm /opt/jdk-23_linux-x64_bin.tar.gz && \
    ln -s /opt/jdk-23.0.1 /opt/java

ENV JAVA_HOME=/opt/jdk-23.0.1
ENV PATH=$JAVA_HOME/bin:$PATH

RUN apt-get install -y sudo adduser
RUN adduser --disabled-password --gecos "" java
RUN apt install git -y
RUN apt install maven -y

RUN wget https://downloads.apache.org/tomcat/tomcat-9/v9.0.96/bin/apache-tomcat-9.0.96.tar.gz
RUN tar -xzvf /opt/apache-tomcat-9.0.96.tar.gz -C /opt
RUN rm /opt/apache-tomcat-9.0.96.tar.gz

RUN sudo su java
RUN git clone https://github.com/la2ki/docker-demo.git
WORKDIR /opt/docker-demo
RUN mvn clean package
RUN sudo chown -R java:java /opt/docker-demo
RUN sudo chown -R java:java /opt/apache-tomcat-9.0.96
RUN sudo chmod +x /opt/apache-tomcat-9.0.96/bin/startup.sh
RUN su - java -c /opt/apache-tomcat-9.0.96/bin/startup.sh