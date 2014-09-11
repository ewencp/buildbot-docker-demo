# # To build:
#
# docker build -t buildbot-master .
#
# # Run buildbot
# CONTAINER_ID=$(docker run -d -p 8010:8010 -v /var/run/docker.sock:/var/run/docker.sock buildbot-master)
#
# The web interface will be available on localhost:8010

FROM ubuntu:14.04

# Install buildbot and its dependencies
RUN apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y python-pip python-dev supervisor
RUN pip install buildbot==0.8.9 docker-py

# You should add buildbot configuration via a volume

# Set supervisord buildbot process
ADD buildbot.conf /etc/supervisor/conf.d/buildbot.conf

# Setup running docker container buildbot process
# Make host port 8010 match container port 8010
EXPOSE 8010
EXPOSE 9989

CMD ["/usr/bin/supervisord", "-n"]
