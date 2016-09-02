FROM bde2020/spark-submit:1.6.2-hadoop2.6

MAINTAINER Cecile Tonglet <cecile.tonglet@tenforce.com>

ENV SPARK_APPLICATION_JAR_NAME application-1.0

COPY template.sh /

RUN  apt-get update \
      && apt-get install -y apt-transport-https \
      && echo "deb https://dl.bintray.com/sbt/debian /" >> /etc/apt/sources.list.d/sbt.list \
      && apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 2EE0EA64E40A89B84B2DF73499E82A75642AC823 \
      && apt-get update \
      && apt-get install -y sbt \
      && chmod +x /template.sh \
      && update-java-alternatives -s java-1.8.0-openjdk-amd64 \
      && mkdir -p /app \
      && mkdir -p /usr/src/app

# Copy the build.sbt first, for separate dependency resolving and downloading
ONBUILD COPY build.sbt /usr/src/app/
ONBUILD COPY project /usr/src/app/project
ONBUILD RUN cd /usr/src/app && sbt update

# Copy the source code and build the application
ONBUILD COPY . /usr/src/app
ONBUILD RUN cd /usr/src/app && sbt clean assembly

CMD ["/bin/bash", "/template.sh"]
