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
RUN cd go-ipfs; git checkout v0.6.0; make install
RUN find / -name ipfs
RUN ipfs version

# Binary Could be copied into another smaller container without build tools at this point
RUN apt-get -y install tini

# Swarm TCP; should be exposed to the public
EXPOSE 4001
# Daemon API; must not be exposed publicly but to client services under you control
EXPOSE 5001
# Web Gateway; can be exposed publicly with a proxy, e.g. as https://ipfs.example.org
EXPOSE 8080
# Swarm Websockets; must be exposed publicly when the node is listening using the websocket transport (/ipX/.../tcp/8081/ws).
EXPOSE 8081

ENV IPFS_PATH /data/ipfs

# The default logging level
ENV IPFS_LOGGING ""

ENTRYPOINT ["/usr/bin/tini", "--", "/go/bin/ipfs"]

# Execute the daemon subcommand by default
CMD ["daemon", "--migrate=true"]