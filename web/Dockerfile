##########################################################################################
# ClassicDriver Docker image with Nexus 3.x
##########################################################################################
FROM classicdriver/java:1.8.60

MAINTAINER Team Nitrous <nitrous@classicdriver.com>

#############################################
# Nexus installation
#############################################

ENV NEXUS_HOME /var/nexus_home
ENV NEXUS_INSTALL_DIR   /opt/sonatype/nexus
ENV NEXUS_VERSION 3.0.0-m6
ENV NEXUS_DOWNLOAD nexus-${NEXUS_VERSION}-bundle.tar.gz
ENV RUN_USER            nexus
ENV RUN_GROUP           nexus

ENV DOWNLOAD_URL        http://download.sonatype.com/nexus/oss/nexus-${NEXUS_VERSION}-bundle.tar.gz
# Nexus is run with user `nexus`, uid = 1000
# If you bind mount a volume from the host or a data container,
# ensure you use the same uid
RUN useradd -c "nexus role account" -d "$NEXUS_HOME" -u 1000 -m -s /bin/bash ${RUN_USER}

# Installation
RUN set -x \
    && mkdir -p                             ${NEXUS_INSTALL_DIR}              \
    && wget                                 ${DOWNLOAD_URL}                   \
    && tar -xzf                             ${NEXUS_DOWNLOAD} --strip=1 -C ${NEXUS_INSTALL_DIR} \
    && rm -rf                               ${NEXUS_DOWNLOAD}                 \
    && chown -R ${RUN_USER}:${RUN_GROUP}    ${NEXUS_INSTALL_DIR}/             \
    && chown -R ${RUN_USER}:${RUN_GROUP}    ${NEXUS_HOME}/

COPY nexus.vmoptions ${NEXUS_INSTALL_DIR}/bin/nexus.vmoptions

USER ${RUN_USER}:${RUN_GROUP}

VOLUME ["${NEXUS_HOME}"]

EXPOSE 8081

WORKDIR $NEXUS_INSTALL_DIR

CMD ["bin/nexus", "run"]
