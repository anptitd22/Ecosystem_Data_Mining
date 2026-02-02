FROM apache/spark:3.5.8-java17

USER root

ENV ICEBERG_VERSION=1.5.0
ENV HADOOP_VERSION=3.4.0

ADD https://repo1.maven.org/maven2/org/apache/iceberg/iceberg-spark-runtime-3.5_2.12/${ICEBERG_VERSION}/iceberg-spark-runtime-3.5_2.12-${ICEBERG_VERSION}.jar /opt/spark/jars/
ADD https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-aws/${HADOOP_VERSION}/hadoop-aws-${HADOOP_VERSION}.jar /opt/spark/jars/


RUN chmod 644 /opt/spark/jars/*.jar

RUN mkdir -p /opt/spark/spark-events && chmod 777 /opt/spark/spark-events

USER spark

WORKDIR /opt/spark