FROM node:10-alpine

WORKDIR /app

ENV PATH /app/node_modules/.bin:$PATH

COPY package*.json ./

RUN apk update

RUN npm install

COPY . .

EXPOSE 80

CMD ["node", "server.js"]
