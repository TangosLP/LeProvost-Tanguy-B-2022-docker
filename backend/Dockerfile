FROM node:10-alpine

WORKDIR /usr/src/app

COPY package*.json ./

RUN apk update

RUN npm install

COPY . .

EXPOSE 80

CMD ["node", "server.js"]
