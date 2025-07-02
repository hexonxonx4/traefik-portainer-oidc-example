# 🔐 Exemple : Portainer derrière Traefik + Keycloak + OAuth2-Proxy

Ce dépôt contient un exemple **fonctionnel et sécurisé** pour déployer un service web (**Portainer**) protégé par **Keycloak (OIDC)**, le tout derrière un reverse proxy **Traefik v3**.

---

## ⚙️ Technologies utilisées

- Traefik v3 (reverse proxy + HTTPS Let's Encrypt)
- Keycloak (authentification centralisée OIDC)
- OAuth2 Proxy (middleware d'authentification)
- Portainer (interface de gestion Docker)

---

## 🚀 Démarrage rapide

### 1. Cloner le dépôt

```bash
git clone https://github.com/hexonxonx4/traefik-portainer-oidc-example.git
cd traefik-portainer-oidc-example
```

### 2. Configurer les variables

```bash
cp .env.example .env
nano .env
```

Ajoute ton `KEYCLOAK_CLIENT_SECRET` généré dans l’interface Keycloak.  
(voir [`keycloak-init.md`](./keycloak-init.md) pour les instructions)

### 3. Lancer les conteneurs

```bash
docker compose up -d
```

---

## 🌐 Accès aux services

| Service      | URL                                       |
|--------------|--------------------------------------------|
| Traefik      | `https://traefik.homelab.local`            |
| Keycloak     | `https://auth.homelab.local`               |
| Portainer    | `https://portainer.homelab.local` (protégé)|
| OAuth2 Proxy | `https://auth-proxy.homelab.local/oauth2`  |

⚠️ Configure ton DNS local ou `/etc/hosts` pour faire pointer ces noms vers ton serveur.

---

## 🧑‍💻 Configuration Keycloak

Voir [`keycloak-init.md`](./keycloak-init.md)

---

## 📚 Ce dépôt est une annexe au guide :
📘 [Sécuriser un homelab moderne](https://github.com/hexonxonx4/homelab-secure-guide)

---

## ✅ Licence

Ce projet est sous licence MIT.
