Example configurations for using Docker with Buildbot.

This configuration adds apt-cacher-ng support so that your builds don't hammer
package mirrors, even if you always run the build from scratch.

Assuming you've checked out this code to `~/buildbot-docker-demo`, build the
Docker images for the master, slaves, and apt-cacher-ng:

    cd ~/buildbot-docker-demo/
    pushd master; docker build -t buildbot-master .; popd
    pushd slave; docker build -t buildbot-slave .; popd
    pushd aptcacherng; docker build -t aptcacherng .; popd

Run the master as with the simple config:

    docker run --rm --name bbmaster -p 8010:8010 \
        -v ~/buildbot-docker-demo/master/data/buildbot/master:/data/buildbot/master \
        -v /var/run/docker.sock:/var/run/docker.sock \
        buildbot-master

Then run the apt-cacher-ng container:

    docker run --rm --name apt-cacher-ng aptcacherng

Since it has a name, we can link to it when creating slave containers. The only
other change required is that we enable proxying through apt-cacher-ng on the
slaves, a simple one line addition to the slave Dockerfile.
