FROM openjdk:8-alpine

RUN apk update && apk add ca-certificates openssl wget unzip

RUN wget -O apache-ctakes-4.0.0-bin.tar.gz http://apache.osuosl.org//ctakes/ctakes-4.0.0/apache-ctakes-4.0.0-bin.tar.gz
RUN tar xzf apache-ctakes-4.0.0-bin.tar.gz

RUN wget -O uima-as-2.9.0-bin.tar.gz http://archive.apache.org/dist/uima//uima-as-2.9.0/uima-as-2.9.0-bin.tar.gz
RUN tar xzf uima-as-2.9.0-bin.tar.gz

RUN mkdir /desc
RUN mkdir /outputs

COPY desc/localDeploymentDescriptor.xml /desc/
COPY desc/remoteFull.xml /desc/
COPY desc/docker-mist.xml /desc/
COPY desc/docker-fast-dictionary.xml /desc/
COPY desc/docker-writer.xml /desc/

ENV UIMA_HOME=/apache-uima-as-2.9.0
ENV CTAKES_HOME=/apache-ctakes-4.0.0
ENV UIMA_LOGGER_CONFIG_FILE=/Logging.properties

COPY ojdbc8.jar /
COPY ctakes-i2b2.jar /
# COPY uimafit-core-2.2.0.jar /
# COPY uimaj-core-2.9.0.jar /
# COPY ctakes-type-system-4.0.0.jar /
# COPY ctakes-core-4.0.0.jar /
# COPY log4j-1.2.17.jar /
# COPY commons-io-2.2.jar /
# COPY commons-logging-1.1.1.jar /
# COPY commons-lang-2.6.jar /
# COPY spring-beans-3.1.2.RELEASE.jar /
# COPY spring-core-3.1.2.RELEASE.jar /
COPY CreateDbReaderDescriptor.java /
COPY MemReleaseI2b2CollectionReader.java /

COPY runRemoteAsyncAE.sh /
COPY wrapper.sh /
COPY Logging.properties /

RUN javac -cp ctakes-i2b2.jar:/apache-ctakes-4.0.0/lib/uimaj-core-2.9.0.jar:/apache-ctakes-4.0.0/lib/uimafit-core-2.2.0.jar:/apache-ctakes-4.0.0/lib/ctakes-type-system-4.0.0.jar:/apache-ctakes-4.0.0/lib/ctakes-core-4.0.0.jar:/apache-ctakes-4.0.0/lib/log4j-1.2.17.jar  CreateDbReaderDescriptor.java MemReleaseI2b2CollectionReader.java
RUN jar cf i2b2.jar *.class

CMD ./wrapper.sh $input_table_name $oracle_host $oracle_user $oracle_pw $broker_host $broker_port
