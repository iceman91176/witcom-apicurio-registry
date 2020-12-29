FROM apicurio/apicurio-registry-streams:1.3.2.Final

ARG THIRD_PARTY_LIBS

ENV APICURIO_HOME=/deployments

RUN adduser apicurio -u 1001 -H -G root -D

#####
# Add 3rd party libs
#####
COPY kafka-thirdparty-libs/${THIRD_PARTY_LIBS}/target/dependency/ ${APICURIO_HOME}/lib/

##non root
USER 1001

