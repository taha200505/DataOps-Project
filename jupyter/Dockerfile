FROM jupyter/pyspark-notebook:latest

USER root

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        openjdk-11-jdk wget tar procps net-tools && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

ENV JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64

# Hadoop client 3.3.6 (identique au cluster)
RUN wget -q https://archive.apache.org/dist/hadoop/common/hadoop-3.3.6/hadoop-3.3.6.tar.gz -O /tmp/hadoop.tar.gz && \
    tar -xzf /tmp/hadoop.tar.gz -C /opt && \
    ln -s /opt/hadoop-3.3.6 /opt/hadoop && \
    rm /tmp/hadoop.tar.gz

# Spark 3.5.1 depuis le tarball officiel (JARs identiques aux workers)
RUN wget -q https://archive.apache.org/dist/spark/spark-3.5.1/spark-3.5.1-bin-hadoop3.tgz -O /tmp/spark.tgz && \
    tar -xzf /tmp/spark.tgz -C /opt && \
    ln -s /opt/spark-3.5.1-bin-hadoop3 /opt/spark && \
    rm /tmp/spark.tgz

# Supprimer le pyspark de PyPI et réinstaller depuis Spark
RUN pip uninstall -y pyspark findspark || true
RUN pip install --no-cache-dir /opt/spark/python
RUN pip install --no-cache-dir faker pandas pyarrow

# JARs HDFS dans le classpath Spark (évite ClassNotFound)
RUN cp /opt/hadoop/share/hadoop/common/hadoop-common-*.jar $SPARK_HOME/jars/ 2>/dev/null || true && \
    cp /opt/hadoop/share/hadoop/hdfs/hadoop-hdfs-client-*.jar $SPARK_HOME/jars/ 2>/dev/null || true

ENV SPARK_HOME=/opt/spark
ENV HADOOP_HOME=/opt/hadoop
ENV HADOOP_CONF_DIR=/opt/hadoop/etc/hadoop
ENV YARN_CONF_DIR=/opt/hadoop/etc/hadoop
ENV PATH=${SPARK_HOME}/bin:${SPARK_HOME}/sbin:${HADOOP_HOME}/bin:${HADOOP_HOME}/sbin:${PATH}

USER ${NB_UID}
WORKDIR /home/jovyan/work