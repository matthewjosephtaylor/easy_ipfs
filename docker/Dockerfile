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
ARG GO_VERSION
RUN apt-get -y install golang-${GO_VERSION}

# Configure Go
ENV GOROOT /usr/lib/go-${GO_VERSION}
ENV GOPATH /go
ENV PATH /usr/lib/go-${GO_VERSION}/bin:/go/bin:$PATH
ENV CGO_ENABLED 0

RUN find / -name go
RUN go version

# Checkout and compile IPFS
WORKDIR /src
RUN git clone https://github.com/ipfs/go-ipfs.git
ARG IPFS_VERSION
RUN cd go-ipfs; git checkout v${IPFS_VERSION}; make install
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

# https://gnanesh.me/avahi-docker-non-root.html
RUN apt-get install -y --no-install-recommends avahi-daemon libnss-mdns \
 avahi-discover avahi-utils iputils-ping dnsutils mdns-scan
RUN printf "[server]\nenable-dbus=no\n" >> /etc/avahi/avahi-daemon.conf \
 && chmod 777 /etc/avahi/avahi-daemon.conf \
 && mkdir -p /var/run/avahi-daemon \
 && chown avahi:avahi /var/run/avahi-daemon \
 && chmod 777 /var/run/avahi-daemon
RUN chmod 7777 /etc/avahi
# COPY entrypoint /app/entrypoint
# ENTRYPOINT ["/app/entrypoint", "/go/bin/ipfs"]
ENTRYPOINT ["/usr/bin/tini", "--", "/go/bin/ipfs"]
# ENTRYPOINT ["bash", "/app/entrypoint"]

# Execute the daemon subcommand by default
CMD ["daemon", "--migrate=true"]