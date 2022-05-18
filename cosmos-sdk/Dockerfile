FROM ubuntu:20.04

RUN apt-get update && apt-get install -y make gcc curl git

RUN curl -OL https://golang.org/dl/go1.17.4.linux-amd64.tar.gz

RUN tar -C /usr/local -xvf go1.17.4.linux-amd64.tar.gz

ENV PATH=$PATH:/usr/local/go/bin

RUN git clone -b v7.0.1  http://github.com/cosmos/gaia
RUN cd gaia && make install

ENV PATH=$PATH:/root/go/bin

RUN echo gaiad version --long
