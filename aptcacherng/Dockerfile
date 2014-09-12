FROM ubuntu:14.04

RUN apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y apt-cacher-ng

EXPOSE 3142

CMD ["/usr/sbin/apt-cacher-ng", "-c", "/etc/apt-cacher-ng", "pidfile=/var/run/apt-cacher-ng/pid", "SocketPath=/var/run/apt-cacher-ng/socket", "foreground=1"]
