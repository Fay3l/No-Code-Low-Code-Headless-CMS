# Utilise l'image officielle Node.js
FROM node:20

# Définir le répertoire de travail
WORKDIR /app

# Copier les fichiers package.json et package-lock.json
COPY package*.json ./

# Installer les dépendances
RUN npm install

# Copier le reste du code source
COPY . .

# Exposer le port Strapi par défaut
EXPOSE 1337

# Lancer Strapi en mode développement (ou "start" pour la prod)
CMD ["npm", "run", "develop"]