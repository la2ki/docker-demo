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

RUN wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.31/bin/apache-tomcat-10.1.31.tar.gz
RUN tar -xzvf /opt/apache-tomcat-10.1.31.tar.gz -C /opt
RUN rm /opt/apache-tomcat-10.1.31.tar.gz

RUN sudo su java
RUN git clone https://github.com/la2ki/docker-demo.git
WORKDIR /opt/docker-demo
RUN mvn clean package
RUN rm -rf /opt/apache-tomcat-10.1.31/webapps
RUN mkdir /opt/apache-tomcat-10.1.31/webapps
RUN cp /opt/docker-demo/target/docker-demo*.war /opt/apache-tomcat-10.1.31/webapps/ROOT.war
RUN sudo chown -R java:java /opt/docker-demo
RUN sudo chown -R java:java /opt/apache-tomcat-10.1.31
RUN sudo chmod +x /opt/apache-tomcat-10.1.31/bin/catalina.sh
USER java
CMD [ "/opt/apache-tomcat-10.1.31/bin/catalina.sh", "run"]