FROM ubuntu:latest
MAINTAINER Nathan Grubb "me@nathangrubb.io"

RUN apt-get update -y
RUN apt-get install -y python3-pip

COPY . /service
WORKDIR /service

RUN pip3 install -r requirements.txt

CMD ["python3","-u","stock.py"]
