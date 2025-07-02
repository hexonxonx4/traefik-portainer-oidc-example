# 🛠️ Configuration Keycloak pour Portainer via OAuth2-Proxy

Ce guide décrit comment configurer un **client OIDC dans Keycloak** pour que `oauth2-proxy` puisse authentifier les accès à Portainer.

---

## ✅ Étapes dans Keycloak (interface Web)

1. **Connexion à l'interface Keycloak**
   - URL : `https://auth.homelab.local`
   - Identifiant : `admin`
   - Mot de passe : `admin` *(ou ce que vous avez défini dans l’environnement)*

2. **Créer un nouveau Realm (facultatif mais recommandé)**
   - Nom : `homelab`
   - ⚠️ Bien se placer dans ce realm avant la suite

3. **Créer un nouveau client OIDC**
   - Menu : Clients → *Créer*
   - ID du client : `portainer`
   - Client Protocol : `openid-connect`
   - Root URL : `https://auth-proxy.homelab.local`

4. **Configurer les détails du client**
   - Accès type : `confidential`
   - Valid Redirect URIs : `https://auth-proxy.homelab.local/oauth2/callback`
   - Web Origins : `*` *(ou spécifique si tu veux restreindre)*
   - Activer le client : ✅

5. **Enregistrer, puis aller dans l'onglet *Credentials***
   - Copier la **Client Secret** dans `.env` :
     ```env
     KEYCLOAK_CLIENT_SECRET=...
     ```

6. **Créer un utilisateur pour tester**
   - Menu : Utilisateurs → *Ajouter utilisateur*
   - Renseigner un email et un mot de passe
   - Activer l'utilisateur
   - Affecter un rôle ou groupe si besoin

---

## 🧪 Validation

- Navigue vers : `https://portainer.homelab.local`
- Tu seras redirigé vers `Keycloak`
- Après authentification, tu accéderas à Portainer

---

## 🔒 Conseils

- Active MFA dans Keycloak pour renforcer l'accès
- Restreins les domaines autorisés dans `OAUTH2_PROXY_EMAIL_DOMAINS`
- Limite les redirect URIs aux domaines légitimes
