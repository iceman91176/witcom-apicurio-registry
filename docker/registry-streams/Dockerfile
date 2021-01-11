ARG APICURIO_TAG=1.3.2.Final
FROM maven:3.6.3-openjdk-11 as builder

ARG THIRD_PARTY_LIBS=2.5.x
ARG APICURIO_RELEASE=1.3.2.Final

RUN mkdir -p /tmp/build /tmp/build/3rdparty

ADD kafka-thirdparty-libs/${THIRD_PARTY_LIBS}/pom.xml /tmp/build/3rdparty/
ADD scripts/mergepom.py /tmp/build/

RUN cd /tmp/build \ 
  && wget https://github.com/Apicurio/apicurio-registry/archive/${APICURIO_RELEASE}.tar.gz \
  && tar -zxf ${APICURIO_RELEASE}.tar.gz \
  && python mergepom.py apicurio-registry-${APICURIO_RELEASE}/storage/streams/pom.xml 3rdparty/pom.xml \
  && cd apicurio-registry-${APICURIO_RELEASE}/storage/streams \
  && mvn clean package -Pprod -DskipTests=true

FROM apicurio/apicurio-registry-streams:${APICURIO_TAG}

ARG APICURIO_RELEASE=1.3.2.Final
ENV APICURIO_HOME=/deployments

RUN adduser apicurio -u 1001 -H -G root -D

COPY --from=builder /tmp/build/apicurio-registry-${APICURIO_RELEASE}/storage/streams/target/apicurio-registry-storage-streams-${APICURIO_RELEASE}-all.tar.gz ${APICURIO_HOME}/

RUN cd ${APICURIO_HOME} \
  && tar -zxf apicurio-registry-storage-streams-${APICURIO_RELEASE}-all.tar.gz \
  && rm apicurio-registry-storage-streams-${APICURIO_RELEASE}-all.tar.gz \
  && chown -Rf apicurio:root *
  
##non root
USER 1001
   



