FROM jupyter/pyspark-notebook:latest

USER root

RUN apt-get update && \
    apt-get install -y \
        openjdk-11-jdk \
        wget \
        tar && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

ENV JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64

# Hadoop client
RUN wget -q \
    https://archive.apache.org/dist/hadoop/common/hadoop-3.3.6/hadoop-3.3.6.tar.gz \
    -O /tmp/hadoop.tar.gz && \
    tar -xzf /tmp/hadoop.tar.gz -C /opt && \
    ln -s /opt/hadoop-3.3.6 /opt/hadoop && \
    rm /tmp/hadoop.tar.gz

ENV HADOOP_HOME=/opt/hadoop
ENV HADOOP_CONF_DIR=/opt/hadoop/etc/hadoop
ENV YARN_CONF_DIR=/opt/hadoop/etc/hadoop

ENV PATH=${HADOOP_HOME}/bin:${HADOOP_HOME}/sbin:${PATH}

# Match Spark exactly
RUN pip install --no-cache-dir \
    pyspark==3.5.1

USER ${NB_UID}

WORKDIR /home/jovyan/work