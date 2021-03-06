FROM debian:stable

MAINTAINER Pier Carlo Chiodi <pierky@pierky.com>

EXPOSE 179

RUN apt-get update && apt-get install -y \
        autoconf \
	bison \
	build-essential \
	curl \
	flex \
	libreadline-dev \
	libncurses5-dev \
	m4 \
	unzip

WORKDIR /root
RUN curl -O -L https://github.com/BIRD/bird/archive/v1.6.3.zip
RUN unzip v1.6.3.zip

# This directory must be mounted as a local volume with '-v `pwd`/bird:/etc/bird:rw' docker's command line option.
# The host's file at `pwd`/bird/bird.conf is used as the configuration file for BIRD.
RUN mkdir /etc/bird

RUN cd bird-1.6.3 && \
	autoconf && \
	./configure && \
	make && \
	make install

RUN cd bird-1.6.3 && \
	autoconf && \
	./configure --enable-ipv6 && \
	make && \
	make install

RUN apt-get install -y \
	libssh-4 \
	libssh-dev \
	cmake

RUN curl -o rtrlib.zip -L https://github.com/rtrlib/rtrlib/archive/v0.3.6.zip
RUN unzip rtrlib.zip

RUN cd rtrlib-0.3.6 && \
	cmake -D CMAKE_BUILD_TYPE=Release . && \
	make && \
	make install

RUN curl -o bird-rtrlib-cli.zip -L https://github.com/rtrlib/bird-rtrlib-cli/archive/v0.1.1.zip
RUN unzip bird-rtrlib-cli.zip

RUN cd bird-rtrlib-cli-0.1.1 && \
	cmake . && \
	make

CMD bird -c /etc/bird/bird.conf -d
