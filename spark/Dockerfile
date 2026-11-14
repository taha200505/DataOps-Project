FROM apache/spark:3.5.1

USER root

RUN apt-get update && \
    apt-get install -y --no-install-recommends curl ca-certificates bzip2 && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Installation de Miniconda avec Python 3.11
RUN curl -sSL https://repo.anaconda.com/miniconda/Miniconda3-py311_24.1.2-0-Linux-x86_64.sh -o /tmp/miniconda.sh && \
    bash /tmp/miniconda.sh -b -p /opt/conda && \
    rm /tmp/miniconda.sh && \
    /opt/conda/bin/conda clean -afy

# Installation de PySpark 3.5.1 (version exacte du cluster) et dépendances
RUN /opt/conda/bin/pip install --no-cache-dir pyspark==3.5.1 pandas pyarrow

# Forcer Spark à utiliser ce binaire Python 3.11
ENV PYSPARK_PYTHON=/opt/conda/bin/python
ENV PYSPARK_DRIVER_PYTHON=/opt/conda/bin/python
ENV PATH="/opt/conda/bin:${PATH}"

USER spark