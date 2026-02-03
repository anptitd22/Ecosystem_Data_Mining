FROM apache/spark:3.5.3-java17

USER root

RUN apt-get update && \
    apt-get install -y python3.11 python3.11-dev python3-pip && \
    update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.11 1 && \
    python3 -m pip install --upgrade pip

ENV PYSPARK_PYTHON=/usr/bin/python3
ENV PYSPARK_DRIVER_PYTHON=/usr/bin/python3

ENV ICEBERG_VERSION=1.10.1
ENV HADOOP_VERSION=3.4.0
ENV SCALA_VERSION=2.12
ENV SPARK_VERSION=3.5

ADD https://repo1.maven.org/maven2/org/apache/iceberg/iceberg-spark-runtime-${SPARK_VERSION}_${SCALA_VERSION}/${ICEBERG_VERSION}/iceberg-spark-runtime-${SPARK_VERSION}_${SCALA_VERSION}-${ICEBERG_VERSION}.jar /opt/spark/jars/
ADD https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-aws/${HADOOP_VERSION}/hadoop-aws-${HADOOP_VERSION}.jar /opt/spark/jars/


RUN chmod 644 /opt/spark/jars/*.jar

RUN mkdir -p /opt/spark/spark-events && chmod 777 /opt/spark/spark-events



USER spark

WORKDIR /opt/spark