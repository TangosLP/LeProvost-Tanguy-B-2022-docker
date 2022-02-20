# LeProvost-Tanguy-B-2022-docker



Ce court fichier explique brièvement le processus de configurations du projet: 



## docker-compose

```
version: "3"
services:


  web:
    build: ./frontend
    depends_on:
      - api
    ports:
      - 3000:3000
    networks:
      - back


  api:
    build: ./backend
    depends_on:
      - mongo
    ports:
      - 8080:8080
    networks: 
      - back


  mongo:
    image: mongo
    restart: always
    volumes: 
      - mongo:/data/mongo
    environment: 
      MONGODB_INITDB_ROOT_USERNAME: tanguy
      MONGODB_INITDB_ROOT_PASSWORD: tanguy

    networks: 
     - back

networks:
  back:

volumes: 
  mongo:
```

---------------------------------------------------------------------------------------------------------------------------------------------------------------------


La version "3" est la version de docker utilisée

Les services utilisés sont au nombre de 3 : 

* web: Le service web ou application web. Son image est build dans la partie frontend, il est dépendant du service "api" qui sera configuré juste après. Les ports du service web par défaut sont 3000:3000 et le nom du réseau par défaut est dans mon cas "- back", on le placera dans chaque service.
* api: Le service api est l'application que nous utiliserons sur notre site web (api rest). L'image est build dans le backend. L'application dépend de la base de données mongo. Les ports de l'api sont laissés par défaut: 8080:8080. On rajoute aussi - back pour lier api au réseau par défaut.
* mongo: Le service de base de données. Il sert à gérer la base de données SQL. On lui attribue le nom qu'on veut, dans mon cas "mongo". L'image utilisée est mongo, on met en place un redémarrage automatique du service, le dossier où mongo sera stocké mongo, on définie ensuite le compte root de base avec ses identifiants. Pour finir, n'oublions de le lier au réseau par défaut (- back).

On définit par la suite le réseau (networks:) en lui donnant un nom (back), qui est rappelé plus haut dans la configuration par "-back".  
On fait de même pour les volumes de stockage des données en lui attribuant un nom (mongo pour moi), il sera aussi rappelé dans la configuration par "mongo".  


---------------------------------------------------------------------------------------------------------------------------------------------------------------------


## Dockerfile du /frontend


```FROM node:10-alpine

WORKDIR /app

ENV PATH /app/node_modules/.bin:$PATH

COPY package*.json ./

RUN apk update

RUN npm install

COPY . .

EXPOSE 3000

CMD ["node", "server.js"]
````

---------------------------------------------------------------------------------------------------------------------------------------------------------------------


**"FROM node:10-alpine"**: Définie l'OS utilisé pour la plateforme node.js et sa version  
**"WORKDIR /app"**: Répertoire de travail de l'application  
**"ENV PATH /app/node_modules/.bin:$PATH"**: Chemin de l'environnement des variables  
**"COPY package.json ./"**: Déplace package.json vers le répertoire antérieur (plus haut dans l'arborescence).  
**"RUN apk update"**: Met à jour les paquets du système alpine du conteneur  
**"RUN npm install"**: Installation du support npm pour node.js  
**"COPY . ."**: Copie les fichiers contenus localement et les colle dans le conteneur docker  
**"EXPOSE 3000"**: Expose le port utilisé par le service web  
**"CMD ["node", "server.js"]"**: Lance les commandes par défaut dans le conteneur docker à son démarrage.  


---------------------------------------------------------------------------------------------------------------------------------------------------------------------


## Dockerfile du /backend

```
FROM node:10-alpine

WORKDIR /usr/src/app

COPY package*.json ./

RUN apk update

RUN npm install

COPY . .

EXPOSE 8080

CMD ["node", "server.js"]
```

---------------------------------------------------------------------------------------------------------------------------------------------------------------------


**"FROM node:10-alpine"**: Définie l'OS utilisé pour la plateforme node.js et sa version  
**"WORKDIR /usr/src/app"**: Répertoire de travail de l'application (backend)  
**"COPY package.json ./"**: Déplace package.json vers le répertoire antérieur (plus haut dans l'arborescence).  
**"RUN apk update"**: Met à jour les paquets du système alpine du conteneur  
**"RUN npm install"**: Installation du support npm pour node.js  
**"COPY . ."**: Copie les fichiers contenus localement et les colle dans le conteneur docker  
**"EXPOSE 8080"**: Expose le port utilisé par le service de l'application api rest  
**"CMD ["node", "server.js"]"**: Lance les commandes par défaut dans le conteneur docker à son démarrage.  

---------------------------------------------------------------------------------------------------------------------------------------------------------------------

Une fois ces fichiers créés dans les bons dossiers (Dockerfile dans /backend et l'autre dans /frontend), retourner dans le dossier racine (où se trouve docker-compose.yml) lancer les conteneur en faisant :  

**docker-compose up** 
