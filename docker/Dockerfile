FROM ubuntu:20.04

# Install Go and other build tools
RUN apt-get update
RUN apt-get -y install git
RUN apt-get -y install make
RUN DEBIAN_FRONTEND="noninteractive" apt-get -y install tzdata
RUN apt-get install -y software-properties-common
RUN add-apt-repository ppa:longsleep/golang-backports
RUN apt-get update

# Need this specific version of golang
RUN apt-get -y install golang-1.14

# Configure Go
ENV GOROOT /usr/lib/go-1.14
ENV GOPATH /go
#ENV PATH /go/bin:$PATH
ENV PATH /usr/lib/go-1.14/bin:/go/bin:$PATH
ENV CGO_ENABLED 0

RUN find / -name go
RUN go version

# https://github.com/ipfs/go-ipfs#download-and-compile-ipfs
WORKDIR /src
RUN git clone https://github.com/ipfs/go-ipfs.git

# TODO make golang/ipfs versions configurable
# Much of this is copied from official IPFS Dockerfile
RUN cd go-ipfs; git checkout v0.6.0; make install
RUN find / -name ipfs
RUN ipfs version

# Binary Could be copied into another container at this point
RUN apt-get -y install tini

# Swarm TCP; should be exposed to the public
EXPOSE 4001
# Daemon API; must not be exposed publicly but to client services under you control
EXPOSE 5001
# Web Gateway; can be exposed publicly with a proxy, e.g. as https://ipfs.example.org
EXPOSE 8080
# Swarm Websockets; must be exposed publicly when the node is listening using the websocket transport (/ipX/.../tcp/8081/ws).
EXPOSE 8081

# Create the fs-repo directory and switch to a non-privileged user.
# ENV IPFS_PATH /data/ipfs
# RUN groupadd -r ipfs
# RUN useradd -m -r -d $IPFS_PATH -g ipfs ipfs
# RUN chown ipfs:ipfs $IPFS_PATH

# Create mount points for `ipfs mount` command
# RUN mkdir /ipfs /ipns \
#   && chown ipfs:users /ipfs /ipns

# Expose the fs-repo as a volume.
# start_ipfs initializes an fs-repo if none is mounted.
# Important this happens after the USER directive so permission are correct.
# VOLUME $IPFS_PATH

# The default logging level
ENV IPFS_LOGGING ""

# This just makes sure that:
# 1. There's an fs-repo, and initializes one if there isn't.
# 2. The API and Gateway are accessible from outside the container.
# ENTRYPOINT ["/sbin/tini", "--", "/usr/local/bin/start_ipfs"]
# ENTRYPOINT ["/usr/bin/tini", "--", "/src/go-ipfs/bin/container_daemon"]
# TODO su to ipfs user / use the standard 'container_daemon' script
ENTRYPOINT ["/usr/bin/tini", "--", "/go/bin/ipfs"]

# Execute the daemon subcommand by default
CMD ["daemon", "--migrate=true"]