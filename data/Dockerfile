##########################################################################################
# ClassicDriver Bitbucket Data Volume Docker Image
##########################################################################################
FROM classicdriver/base

MAINTAINER Team Nitrous <nitrous@classicdriver.com>

# Bitbucket User
ENV RUN_USER            bitbucket

# https://confluence.atlassian.com/display/BitbucketServer/Bitbucket+Server+home+directory
ENV BITBUCKET_HOME          /var/bitbucket_home

RUN useradd -c "bitbucket role account" -d "$BITBUCKET_HOME" -u 1000 -m -s /bin/bash ${RUN_USER}

VOLUME ["${BITBUCKET_HOME}"]

# Switching user to 'bitbucket' (since this is a data valume and will be used by the bitbucket application container user)
USER ${RUN_USER}

# Command runned once at startup
CMD ["echo", "Data Volume Container for Bitbucket Server"]
