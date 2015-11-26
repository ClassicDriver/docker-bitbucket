##########################################################################################
# ClassicDriver Docker image with Bitbucket installed (version 3.x)
##########################################################################################
FROM classicdriver/debian-wheezy-base

MAINTAINER Team Nitrous <nitrous@classicdriver.com>

#############################################
# Bitbucket installation
#############################################

# Use the default unprivileged account. This could be considered bad practice
# on systems where multiple processes end up being executed by 'daemon' but
# here we only ever run one process anyway.
ENV RUN_USER            bitbucket

# https://confluence.atlassian.com/display/BitbucketServer/Bitbucket+Server+home+directory
ENV BITBUCKET_HOME          /var/bitbucket_home

RUN useradd -c "bitbucket role account" -d "$BITBUCKET_HOME" -u 1000 -m -s /bin/bash ${RUN_USER}

VOLUME ["${BITBUCKET_HOME}"]

# Switching user to 'jenkins' (since this is a data valume and will be used by the jenkins application container user)
USER ${RUN_USER}

# Command runned once at startup
CMD ["echo", "Data container for Bitbucket Server"]
