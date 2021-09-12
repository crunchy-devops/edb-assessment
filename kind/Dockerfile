FROM ubuntu

RUN mkdir /app
WORKDIR /app
COPY . /app

RUN apt-get update && \
    apt-get -y install python3 python3-pip vim iputils-ping && \
    pip3 install -r requirements.txt

EXPOSE 8000


