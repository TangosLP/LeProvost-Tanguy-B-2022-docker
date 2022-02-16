FROM alpine:latest

WORKDIR /usr/src/app

COPY package*.json ./

RUN apk update

RUN npm install

EXPOSE 8888
