# Étape 1 : Build de l'application Vue.js
FROM node:20 AS build

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .
RUN npm run build

# Étape 2 : Serveur web pour les fichiers statiques
FROM nginx:alpine

# Copier le build dans le dossier public de nginx
COPY --from=build /app/dist /usr/share/nginx/html

# Exposer le port par défaut de nginx
EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]