# leap-control
Docker image configuration for a basic control system stack based on Sardana & Tango on openSUSE Leap.

# How to run it?
Run it in detached mode and later on execute bash in order to play with it.

~~~~
docker run -d --name=leap-control -v /sys/fs/cgroup:/sys/fs/cgroup:ro \
           -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix --cap-add=SYS_PTRACE \
           reszelaz/leap-control
docker exec -it leap-control bash
~~~~

Some command details:
* cgroups directory is mounted so the systemd can launch the services e.g. mysql (see [link](http://vpavlin.eu/2015/02/fedora-docker-and-systemd/) for more details)
* In order to use the host's X server (be able to run GUI) we need to export the DISPLAY environment variable and mount the X11 socket directory (see [link](https://hub.docker.com/r/cpascual/taurus-test/) for more details).
* SYS_PTRACE capability is added so the init scripts can run the daemon process with user different than root (see [link](https://github.com/docker/docker/issues/6800) for more details)

# How to start the control system?

~~~~
# start the mysql database
systemctl start mysql
# create the tando database in the mysql
/usr/share/tango-db/create_db.sh
# start the DataBaseds server
systemctl start tango-db
# start the Starter server
systemctl start tango-starter
~~~~
