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
git clone https://github.com/ton-utilisateur/traefik-portainer-oidc-example.git
cd traefik-portainer-oidc-example
```

### 2. Renseigne les variables :

Crée un fichier `.env` :

```env
KEYCLOAK_CLIENT_ID=portainer
KEYCLOAK_CLIENT_SECRET=colle-ton-secret-ici
PORTAINER_HOSTNAME=portainer.homelab.local
OAUTH2_PROXY_HOSTNAME=auth-proxy.homelab.local
OAUTH2_PROXY_COOKIE_SECRET=w93NlUOzG7V2ZpbjmdCOc1f6Fup1N4cY2jAnz9ncuVc=
LETSENCRYPT_EMAIL=ton@email.com
```

#### 🔑 Générer un `OAUTH2_PROXY_COOKIE_SECRET` valide :

```bash
openssl rand -base64 32
```

Le résultat doit être **exactement 32 bytes encodés en base64** (43 caractères).

---

## 🔁 Initialisation de Keycloak

Voir [`keycloak-init.md`](./keycloak-init.md) pour la configuration manuelle ou automatique via import JSON.

---

## ⚠️ Limitations et pièges courants

### ❗Portainer — pas besoin de `--external-auth`

Il suffit de démarrer avec :

```yaml
command:
  - -H unix:///var/run/docker.sock
```

### ❗Traefik — domaine `.local` incompatible avec Let’s Encrypt

Let’s Encrypt ne délivre pas de certificat pour `*.local`.

**Solutions :**

- Soit utiliser `tls=true` dans les routeurs sans certificat auto
- Soit utiliser des certificats auto-signés (ex: `mkcert`)
- Soit utiliser un domaine réel dans `/etc/hosts` (ex: `*.homelab.lan` ou `*.test`)

### ❗oauth2-proxy : port non exposé

Ajoute dans le service :

```yaml
expose:
  - "4180"
```

---

## 📐 Accès

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
