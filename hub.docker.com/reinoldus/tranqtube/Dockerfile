############################################################
# Dockerfile to build Python WSGI Application Containers
# Based on Ubuntu/Debia
############################################################
FROM python:latest

MAINTAINER Stefan Hesse

ENV TZ=Europe/Berlin
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN useradd --create-home --shell /bin/bash flask && echo "flask:flask" | chpasswd && adduser flask sudo && passwd -d flask

RUN apt-get update && apt-get install -y apt-transport-https curl software-properties-common
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
RUN curl -sL https://deb.nodesource.com/setup_8.x | bash -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list
RUN apt-get update && apt-get -y install yarn npm nodejs

USER flask

ENV PATH=$PATH:/home/flask/.local/bin

ADD /deploy-requirements.txt /home/flask/tranqtube/requirements.txt

RUN pip3 install --user -r /home/flask/tranqtube/requirements.txt

# Has to be here, otherwise app folder is root user?!
COPY /tranqtube /home/flask/tranqtube/tranqtube/
COPY /tranqtube/boot.py /home/flask/tranqtube/
COPY /package.json /home/flask/tranqtube/

USER root

WORKDIR /home/flask/tranqtube

RUN yarn install
RUN mkdir -p ./tranqtube/static/jquery/ ./tranqtube/static/bootstrap/ ./tranqtube/static/video.js/
RUN cp -R ./node_modules/jquery/dist/* ./tranqtube/static/jquery/
RUN cp -R ./node_modules/bootstrap/dist/* ./tranqtube/static/bootstrap/
RUN cp -R ./node_modules/video.js/dist/* ./tranqtube/static/video.js/
RUN rm -rf ./node_modules/

RUN chown flask:flask -R /home/flask/

RUN apt-get -y remove yarn npm nodejs && apt-get autoremove

USER flask

ENV FLASK_APP=tranqtube

# CMD gunicorn -k 'eventlet' -b 0.0.0.0:8000 boot:app

EXPOSE 8000

CMD gunicorn -b 0.0.0.0:8000 boot:app