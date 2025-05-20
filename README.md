# CMS Headless – Strapi & Vue.js

Ce projet est une application web moderne utilisant **Strapi** comme backend headless CMS et **Vue.js** comme frontend SPA.

---

## Fonctionnement

### Strapi (Backend)
- Strapi est un CMS headless open-source basé sur Node.js.
- Il permet de créer des modèles de données (ex : films, réalisateurs, genres) et de gérer le contenu via une interface d’administration.
- Strapi expose automatiquement une API REST sécurisée pour consommer les données.
- Gestion intégrée des utilisateurs, rôles et permissions.

### Vue.js (Frontend)
- Vue.js est un framework JavaScript pour créer des interfaces utilisateur réactives.
- L’application Vue consomme l’API Strapi pour afficher et gérer les données (liste des films, détails, inscription, connexion, etc.).
- Authentification via JWT : le token est stocké dans le navigateur et envoyé dans les requêtes API.

---

## Installation du projet

### 1. Cloner le dépôt

```bash
git clone <url-du-repo>
cd CMS\ Headless
```

### 2. Installer et lancer le backend Strapi

```bash
cd strapi-controle
npm install
npm run develop
```
Accès à l’admin Strapi : http://localhost:1337/admin
3. Installer et lancer le frontend Vue.js

```bash
cd vue-project
npm install
npm run dev
```

Accès à l’application : http://localhost:5173

#### Utilisation
Créer/modifier du contenu : via l’interface d’administration Strapi.
Consommer l’API : l’application Vue.js utilise axios pour interagir avec Strapi.
Authentification : inscription et connexion via l’API Strapi, token JWT stocké dans le navigateur.
Documentation
Strapi Documentation
Vue.js Documentation
Auteur
Projet réalisé avec Strapi & Vue.js.

