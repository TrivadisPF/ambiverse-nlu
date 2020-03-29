FROM maven:3.6.0-jdk-8
# Forked from https://github.com/ambiverse-nlu/dockerfiles
MAINTAINER AmbiverseNLU <ambiversenlu-admin@mpi-inf.mpg.de>
MAINTAINER Philip Krauss <philip.krauss@trivadis.com>

ENV AIDA_CONF=default

WORKDIR /ambiverse-nlu

RUN git clone https://github.com/TrivadisPF/ambiverse-nlu.git /ambiverse-nlu

RUN sed -i '/^log4j.appender.FILE.File=.*/c\log4j.appender.FILE.File=/ambiverse-nlu/logs/ambiverse-nlu.log' /ambiverse-nlu/src/main/resources/log4j.properties

RUN sed -i '/^log4j.appender.requestLog.File=.*/c\log4j.appender.requestLog.File=/ambiverse-nlu/logs/requests.log' /ambiverse-nlu/src/main/resources/log4j.properties

ADD cassandra-init.sh /cassandra-init.sh

#Download cassandra to be able to use cqlsh and sstableload to create namespaces and load data on the remote server.
ENV CASSANDRA_VERSION=3.11.4
ENV CASSANDRA_PATH="cassandra/${CASSANDRA_VERSION}/apache-cassandra-${CASSANDRA_VERSION}-bin.tar.gz"
ENV CASSANDRA_DOWNLOAD="http://www.apache.org/dyn/closer.cgi?path=/${CASSANDRA_PATH}&as_json=1"
ENV CASSANDRA_MIRROR = "https://archive.apache.org/dist/cassandra/3.11.4/apache-cassandra-3.11.4-bin.tar.gz"

RUN set -x \
  && mkdir -p /cassandra \
  && mkdir -p /var/tmp/data \
  && echo "Downloading cassandra " \
  && wget -q -O - $CASSANDRA_MIRROR | tar -xzf - -C /usr/local/

ENV MAVEN_OPTS -Djetty.port=8080 -Xmx100G -Dorg.eclipse.jetty.annotations.maxWait=1800
ENTRYPOINT ["mvn", "jetty:run"]

EXPOSE 8080