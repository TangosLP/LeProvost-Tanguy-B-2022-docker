FROM node:14-alpine

WORKDIR /usr/src/app

COPY package*.json ./

RUN apk update

RUN npm install

COPY . .

EXPOSE 8888
