FROM java:8-alpine

ENV HADOOP_VERSION=2.7 \
    SCALA_VERSION=2.12.7 \
    SCALA_HOME=/usr/local/share/scala \
    SPARK_PATH=/usr/local/spark \
    SPARK_VERSION=2.3.2 \
    SBT_PATH=/usr/local/sbt \
    SBT_VERSION=1.2.6 \
    PATH=$PATH:"$SPARK_PATH"/bin \
    PATH=$PATH:"$SBT_PATH/bin"

RUN mkdir -p "$SPARK_PATH" \
             "$SBT_PATH" \
             "$SCALA_HOME"

RUN set -x \
    && apk update \
    && apk add --no-cache --update \
        bash \
        tar \
        ca-certificates \
        openssl \
        curl \
    && update-ca-certificates

WORKDIR /tmp

# Apache Spark
RUN wget "http://apache.cs.utah.edu/spark/spark-${SPARK_VERSION}/spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz" \
    && tar -xzf "spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz" --directory "$SPARK_PATH" --strip-components 1 \
    && rm "spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz"

# Scala
RUN curl "http://downloads.lightbend.com/scala/${SCALA_VERSION}/scala-${SCALA_VERSION}.tgz" > scala-${SCALA_VERSION}.tgz \
    && tar -xzf "scala-${SCALA_VERSION}.tgz" --directory "$SCALA_HOME" --strip-components 1 \
    && rm -rf "/tmp/"*

ENV PATH "$PATH:$SCALA_HOME/bin"

# SBT
RUN wget "https://piccolo.link/sbt-${SBT_VERSION}.tgz" \
    && tar -xzf "sbt-${SBT_VERSION}.tgz" --directory "/usr/local" \
    && ln -s "${SBT_PATH}/bin/"* "/usr/bin/" \
    && rm "sbt-${SBT_VERSION}.tgz" \
    && sbt sbtVersion

WORKDIR /app

ENTRYPOINT [ "ash" ]
