FROM java:openjdk-8-jdk

ENV hadoop_ver 2.7.7
ENV spark_ver 2.4.5

# Install python 3

# RUN sed -i '/jessie-updates/d' /etc/apt/sources.list
# RUN apt-get -o Acquire::Check-Valid-Until=false update

# RUN echo "deb [check-valid-until=no] http://cdn-fastly.deb.debian.org/debian jessie main" > /etc/apt/sources.list.d/jessie.list
# RUN echo "deb [check-valid-until=no] http://archive.debian.org/debian jessie-backports main" > /etc/apt/sources.list.d/jessie-backports.list
# RUN sed -i '/deb http:\/\/deb.debian.org\/debian jessie-updates main/d' /etc/apt/sources.list
# RUN apt-get -o Acquire::Check-Valid-Until=false update

RUN echo "deb [check-valid-until=no] http://archive.debian.org/debian jessie-backports main" > /etc/apt/sources.list.d/jessie-backports.list
# As suggested by a user, for some people this line works instead of the first one. Use whichever works for your case
RUN echo "deb [check-valid-until=no] http://archive.debian.org/debian jessie main" > /etc/apt/sources.list.d/jessie.list
RUN sed -i '/deb http:\/\/deb.debian.org\/debian jessie-updates main/d' /etc/apt/sources.list

RUN apt-get -o Acquire::Check-Valid-Until=false update

# RUN apt-get install -y software-properties-common
# RUN add-apt-repository ppa:deadsnakes/ppa

# RUN apt-get install -y python3.7 python3.7-pip

RUN apt-get install -y build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libreadline-dev libffi-dev
RUN curl -O https://www.python.org/ftp/python/3.7.3/Python-3.7.3.tar.xz
RUN tar -xf Python-3.7.3.tar.xz && \
    cd Python-3.7.3 && \
    ./configure --enable-optimizations && \
    make install

RUN mkdir -p /opt && \
    cd /opt && \
    curl http://archive.apache.org/dist/hadoop/common/hadoop-${hadoop_ver}/hadoop-${hadoop_ver}.tar.gz | \
        tar -zx && \
    ln -s hadoop-${hadoop_ver} hadoop && \
    echo Hadoop ${hadoop_ver} installed in /opt

RUN mkdir -p /opt && \
    cd /opt && \
    curl http://archive.apache.org/dist/spark/spark-${spark_ver}/spark-${spark_ver}-bin-without-hadoop.tgz | \
        tar -zx && \
    ln -s spark-${spark_ver}-bin-without-hadoop spark && \
    echo Spark ${spark_ver} installed in /opt

ENV SPARK_HOME=/opt/spark
ENV PATH=$PATH:$SPARK_HOME/bin
ENV HADOOP_HOME=/opt/hadoop
ENV PATH=$PATH:$HADOOP_HOME/bin
ENV LD_LIBRARY_PATH=$HADOOP_HOME/lib/native

ENV PYSPARK_PYTHON=python3

RUN curl https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-aws/2.8.2/hadoop-aws-2.8.2.jar -o /opt/spark/jars/hadoop-aws-2.8.2.jar
RUN curl https://repo1.maven.org/maven2/org/apache/httpcomponents/httpclient/4.5.3/httpclient-4.5.3.jar -o /opt/spark/jars/httpclient-4.5.3.jar
RUN curl https://repo1.maven.org/maven2/joda-time/joda-time/2.9.9/joda-time-2.9.9.jar -o /opt/spark/jars/joda-time-2.9.9.jar
RUN curl https://repo1.maven.org/maven2/com/amazonaws/aws-java-sdk-core/1.11.712/aws-java-sdk-core-1.11.712.jar -o /opt/spark/jars/aws-java-sdk-core-1.11.712.jar
RUN curl https://repo1.maven.org/maven2/com/amazonaws/aws-java-sdk/1.11.712/aws-java-sdk-1.11.712.jar -o /opt/spark/jars/aws-java-sdk-1.11.712.jar
RUN curl https://repo1.maven.org/maven2/com/amazonaws/aws-java-sdk-kms/1.11.712/aws-java-sdk-kms-1.11.712.jar -o /opt/spark/jars/aws-java-sdk-kms-1.11.712.jar
RUN curl https://repo1.maven.org/maven2/com/amazonaws/aws-java-sdk-s3/1.11.712/aws-java-sdk-s3-1.11.712.jar -o /opt/spark/jars/aws-java-sdk-s3-1.11.712.jar

ADD start-common.sh start-worker start-master /
ADD core-site.xml /opt/spark/conf/core-site.xml
ADD spark-defaults.conf /opt/spark/conf/spark-defaults.conf
ENV PATH $PATH:/opt/spark/bin
