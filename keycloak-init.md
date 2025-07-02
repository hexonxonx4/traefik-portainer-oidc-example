# 🛠️ Initialisation de Keycloak (automatique)

Ce dépôt configure Keycloak **automatiquement** à l'aide d'un fichier JSON importé au démarrage.

Le fichier [`keycloak-homelab-realm.json`](./keycloak-homelab-realm.json) contient :

- Le realm `homelab`
- Un client OIDC `portainer`
- Un utilisateur admin `admin / changeme`

---

## ✏️ Personnalisation

Tu peux modifier :

- Le nom du realm
- Le secret du client
- Les utilisateurs créés

---

## 🔁 À chaque redémarrage

L’import **ne se répète pas** : une fois initialisé, Keycloak conserve sa base de données.
