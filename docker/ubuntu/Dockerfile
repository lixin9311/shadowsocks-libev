FROM ubuntu:latest

MAINTAINER Sah Lee <contact@leesah.name>

ENV DEPENDENCIES git-core build-essential autoconf libtool libssl-dev asciidoc xmlto unzip wget libpcre3-dev
ENV BASEDIR /tmp/shadowsocks-libev
ENV SERVER_PORT 8338

# Set up building environment
RUN apt-get update \
 && apt-get --no-install-recommends install -y $DEPENDENCIES

# Get the latest code, build and install
# RUN git clone http://github.com/shadowsocks/shadowsocks-libev.git $BASEDIR
WORKDIR $BASEDIR
RUN wget https://github.com/shadowsocks/shadowsocks-libev/archive/master.zip -O shadowsocks-libev-master.zip --no-check-certificate
RUN unzip shadowsocks-libev-master.zip
RUN mv ./shadowsocks-libev-master/* ./
RUN ./configure \
 && make \
 && make install

# Tear down building environment and delete git repository
WORKDIR /
RUN rm -rf $BASEDIR/shadowsocks-libev\
 && apt-get --purge autoremove -y $DEPENDENCIES

# Port in the config file won't take affect. Instead we'll use 8388.
EXPOSE $SERVER_PORT
EXPOSE $SERVER_PORT/udp

# Override the host and port in the config file.
ADD entrypoint /
ENTRYPOINT ["/entrypoint"]
CMD ["-h"]
