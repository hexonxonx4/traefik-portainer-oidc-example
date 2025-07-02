# 🔐 Exemple : Portainer sécurisé par Keycloak via Traefik + oauth2-proxy

Ce dépôt contient un exemple minimal de stack sécurisée avec :

- **Portainer** comme interface de gestion Docker
- **Traefik** comme reverse proxy
- **Keycloak** comme fournisseur OIDC
- **oauth2-proxy** comme middleware d’authentification

---

## 📐 Architecture

![Architecture](./architecture.png)

---

## 🚀 Démarrage rapide

### 1. Clone le dépôt

```bash
git clone https://github.com/hexonxonx4/traefik-portainer-oidc-example.git
cd traefik-portainer-oidc-example
```

### 2. Renseigne les variables :

Crée un fichier `.env` :

```env
KEYCLOAK_CLIENT_ID=portainer
KEYCLOAK_CLIENT_SECRET=colle-ton-secret-ici
PORTAINER_HOSTNAME=portainer.homelab.local
OAUTH2_PROXY_HOSTNAME=auth-proxy.homelab.local
```

Si tu utilises l’import automatique, tu n’as pas besoin de `KEYCLOAK_CLIENT_SECRET`.

---

## 🔁 Initialisation de Keycloak

### 🔧 Option A — Manuelle (UI)

Crée manuellement le client `portainer` dans Keycloak (voir `keycloak-init.md`).

### ⚡ Option B — Import JSON automatique

Un fichier de configuration (`keycloak-homelab-realm.json`) est fourni pour injecter automatiquement :

- le realm `homelab`
- le client OIDC `portainer`
- un utilisateur admin `admin / changeme`

Tu peux activer cela via ce volume dans le `docker-compose.yml` :

```yaml
volumes:
  - ./keycloak-homelab-realm.json:/opt/keycloak/data/import/realm.json:ro
command: >
  start-dev --import-realm
```

---

## 🔐 Accès

- https://portainer.homelab.local → redirige vers Keycloak
- https://auth-proxy.homelab.local/oauth2/callback → utilisé automatiquement par oauth2-proxy

---

## 📁 Arborescence

```
.
├── docker-compose.yml
├── .env
├── keycloak-homelab-realm.json
├── keycloak-init.md
└── architecture.png
```

---

## 📜 Licence

MIT — à utiliser comme base pour homelabs ou formations. Pas encore conçu pour une mise en production telle quelle.
