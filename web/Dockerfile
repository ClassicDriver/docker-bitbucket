##########################################################################################
# ClassicDriver Docker image with Bitbucket
##########################################################################################
FROM classicdriver/java:1.8.60

MAINTAINER Team Nitrous <nitrous@classicdriver.com>

#############################################
# Bitbucket installation
#############################################

# Install git, download and extract Bitbucket Server and create the required directory layout.
# Try to limit the number of RUN instructions to minimise the number of layers that will need to be created.
RUN echo "deb http://http.debian.net/debian wheezy-backports main" >/etc/apt/sources.list.d/wheezy-backports.list \
    && apt-get update -qq \
    && DEBIAN_FRONTEND=noninteractive apt-get -t wheezy-backports install -y -qq git \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends libtcnative-1 \
    && apt-get clean autoclean \
    && apt-get autoremove --yes \
    && rm -rf /var/lib/{apt,dpkg,cache,log}/

# Use the default unprivileged account. This could be considered bad practice
# on systems where multiple processes end up being executed by 'daemon' but
# here we only ever run one process anyway.
ENV RUN_USER            bitbucket
ENV RUN_GROUP           bitbucket

# https://confluence.atlassian.com/display/BitbucketServer/Bitbucket+Server+home+directory
# ENV BITBUCKET_HOME          /var/atlassian/application-data/bitbucket
ENV BITBUCKET_HOME          /var/bitbucket_home

# Bitbucket is run with user `bitbucket`, uid = 1000
# If you bind mount a volume from the host or a data container,
# ensure you use the same uid
RUN useradd -c "bitbucket role account" -d "$BITBUCKET_HOME" -u 1000 -m -s /bin/bash ${RUN_USER}

# Install Atlassian Bitbucket Server to the following location
ENV BITBUCKET_INSTALL_DIR   /opt/atlassian/bitbucket

ENV BITBUCKET_VERSION 4.1.0
ENV DOWNLOAD_URL        https://downloads.atlassian.com/software/stash/downloads/atlassian-bitbucket-${BITBUCKET_VERSION}.tar.gz

RUN mkdir -p                             ${BITBUCKET_INSTALL_DIR} \
    && curl -L --silent                  ${DOWNLOAD_URL} | tar -xz --strip=1 -C "$BITBUCKET_INSTALL_DIR" \
    && mkdir -p                          ${BITBUCKET_INSTALL_DIR}/conf/Catalina      \
    && chmod -R 700                      ${BITBUCKET_INSTALL_DIR}/conf/Catalina      \
    && chmod -R 700                      ${BITBUCKET_INSTALL_DIR}/logs               \
    && chmod -R 700                      ${BITBUCKET_INSTALL_DIR}/temp               \
    && chmod -R 700                      ${BITBUCKET_INSTALL_DIR}/work               \
    && chown -R ${RUN_USER}:${RUN_GROUP} ${BITBUCKET_INSTALL_DIR}/                   \
    && ln --symbolic                     "/usr/lib/x86_64-linux-gnu/libtcnative-1.so" "${BITBUCKET_INSTALL_DIR}/lib/native/libtcnative-1.so"

USER ${RUN_USER}:${RUN_GROUP}

VOLUME ["${BITBUCKET_HOME}"]

# HTTP Port
EXPOSE 7990

# SSH Port
EXPOSE 7999

WORKDIR $BITBUCKET_INSTALL_DIR

# Run in foreground
CMD ["./bin/start-bitbucket.sh", "-fg"]
