FROM ewencp/dind

# Install buildbot, its dependencies, and dependencies for builds
RUN apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y python-pip python-dev supervisor \
                                   git
RUN pip install buildbot-slave==0.8.9

# Use apt-cacher-ng, assuming we're linked to container 'apt-cacher-ng'
ADD aptcacherng /etc/apt/apt.conf.d/apt-cacher-ng

# Create buildbot configuration
RUN mkdir -p /data/buildbot/slave
ADD buildbot.tac /data/buildbot/slave/buildbot.tac
ADD info/admin /data/buildbot/slave/info/admin
ADD info/host /data/buildbot/slave/info/host

# Service
ADD buildbot.conf /etc/supervisor/conf.d/buildbot.conf

CMD ["wrapdocker", "/usr/bin/supervisord", "-n"]
