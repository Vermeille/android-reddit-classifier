FROM ubuntu:15.10

ENV CC gcc
ENV CXX  g++

# What is the best practice regarding workdir?
WORKDIR /root

# TODO: this was intended as a very general dev platform, but the http interface doesn't need
# all of this. Purge the unnecessary
RUN apt-get update &&  apt-get install -y \
         automake \
         autoconf \
         autoconf-archive \
         cmake \
         gcc \
         g++ \
         git \
         libboost-all-dev \
         libgoogle-glog-dev \
         libgflags-dev \
         make \
         pkg-config

RUN ln -s /usr/bin/aclocal-1.15 /usr/bin/aclocal-1.14
RUN ln -s /usr/bin/automake-1.15 /usr/bin/automake-1.14

# Http Interface runs on GNU libmicrohttpd
RUN git clone https://github.com/Metaswitch/libmicrohttpd --depth=1 && \
    cd libmicrohttpd && \
    chmod +x ./configure && \
    ./configure --enable-doc=no && make && make install && \
    cd .. && \
    rm -rf libmicrohttpd

RUN apt-get install -y gdb valgrind

RUN git clone https://github.com/Vermeille/http-interface && \
    cd http-interface && \
    git checkout fcde45e053315 && \
    mkdir build && \
    cd build && \
    cmake .. && \
    make && \
    make install && \
    cd ../.. && \
    rm -rf http-interface

RUN git clone https://github.com/Vermeille/nlp-common && \
    cd nlp-common && \
    git checkout 0a14978 && \
    mkdir build && \
    cd build && \
    cmake .. && \
    make && \
    make install && \
    cd ../.. && \
    rm -rf nlp-common

RUN git clone https://github.com/Vermeille/bow && \
    cd bow && \
    git checkout 6ffe498 && \
    mkdir build && \
    cd build && \
    cmake .. && \
    make && \
    cd ../..

# Mandatory: generate the locales of the languages the library uses.
RUN locale-gen fr_FR.UTF-8

ADD . /root

EXPOSE 8888

ENTRYPOINT ["./bow/build/bow"]
