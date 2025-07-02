# 🎛️ Configuration de Keycloak pour l'exemple Portainer + Traefik + OAuth2-Proxy

Ce guide t'explique deux façons de configurer un **client OIDC confidentiel** dans Keycloak, compatible avec `oauth2-proxy`.

---

## 🧰 Option A : Interface Web (manuelle)

1. Connecte-toi à Keycloak (ex: https://auth.homelab.local)
2. Va à **Clients > Create client**
3. Renseigne :
   - **Client ID** : `portainer`
   - **Client type** : `OpenID Connect`
   - **Root URL** : `https://portainer.homelab.local`
4. Clique **Next**
5. Active :
   - ✅ **Client authentication**
6. Dans **Redirect URIs**, ajoute :
   ```
   https://auth-proxy.homelab.local/oauth2/callback
   ```
7. Dans **Web Origins**, ajoute :
   ```
   *
   ```
8. Enregistre et récupère le **Client Secret** dans l'onglet Credentials

---

## ⚡ Option B : Automatisée (import JSON)

Cette option permet de préconfigurer Keycloak automatiquement au démarrage.

### ✅ Prérequis

- Le fichier [`keycloak-homelab-realm.json`](./keycloak-homelab-realm.json) doit exister
- Le `docker-compose.yml` doit monter ce fichier comme volume :

```yaml
  keycloak:
    ...
    volumes:
      - keycloak_data:/opt/keycloak/data
      - ./keycloak-homelab-realm.json:/opt/keycloak/data/import/realm.json:ro
    command: >
      start-dev --import-realm
```

### ✅ Ce que ça configure

- Realm `homelab`
- Client `portainer` (confidential)
- Redirect URI : `https://auth-proxy.homelab.local/oauth2/callback`
- Utilisateur admin : `admin / changeme`

---

## 📥 Variables à insérer dans `.env`

```env
KEYCLOAK_CLIENT_ID=portainer
KEYCLOAK_CLIENT_SECRET=colle-le-ici (si mode manuel)
```

---

Utilise l'import JSON pour accélérer les tests locaux ou CI/CD. Pour une prod, vérifie les permissions et renforce les mots de passe.
