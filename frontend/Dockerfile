FROM alpine:latest

WORKDIR /app

ENV PATH /app/node_modules/.bin:$PATH

COPY package*.json ./

RUN apk update

RUN npm install

EXPOSE 8000
