FROM ubuntu:18.04

RUN apt-get update
RUN apt-get install -y wget

RUN wget https://github.com/bbuchfink/diamond/releases/download/v0.9.22/diamond-linux64.tar.gz
RUN tar xzf diamond-linux64.tar.gz

ENV PATH=/:$PATH

