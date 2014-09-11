Example configurations for using Docker with Buildbot.

This simple configuration just demonstrates how to use Docker containers,
allocated on-demand, for build slaves. This makes it easy to get true,
completely-from-scratch builds and testing.

Assuming you've checked out this code to `~/buildbot-docker-demo`, build the
Docker images for the master and for slaves:

    cd ~/buildbot-docker-demo/
    pushd master; docker build -t buildbot-master .; popd
    pushd slave; docker build -t buildbot-slave .; popd

Run the master.

    docker run --rm --name bbmaster -p 8010:8010 \
        -v ~/buildbot-docker-demo/master/data/buildbot/master:/data/buildbot/master \
        -v /var/run/docker.sock:/var/run/docker.sock \
        buildbot-master

This:

* is named so that we can use linking to provide access to the slaves (in master.cfg)
* exposes port 8010 on the host so you can see the web interface
* has a volume mounted for the master's configuration + data
* mounts the Docker socket

Given a slightly modified config and command you could put masters and slaves on
different servers.

* Don't use naming but expose port 9989 as well (used by slaves to communicate
  with master)
* Enable Docker API over HTTP on slave hosts so new slave containers can be
  created by the master on the slave host.

Now, you can use the web interface to trigger some builds (username: buildbot,
password: password). The demo project is a small Django app that includes tests
and requires installing additional system packages. The base docker image for
the slave only includes the base Ubuntu image, the buildbot slave, and git.
