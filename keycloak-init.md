# 🎛️ Configuration de Keycloak pour l'exemple Portainer + Traefik + OAuth2-Proxy

Ce guide t'explique comment créer un **client OIDC confidentiel** dans Keycloak (v22+), compatible avec `oauth2-proxy`.

---

## 🧱 Pré-requis

- Un Realm existant (par défaut : `master`)
- Un utilisateur (admin) connecté à l’interface Keycloak
- Ton instance Keycloak accessible à `https://auth.homelab.local`

---

## 🆕 Étapes pour créer un client confidentiel

1. Dans la console d'administration, va à **Clients** > **Create client**

2. Renseigne :
   - **Client ID** : `portainer`
   - **Client type** : `OpenID Connect`
   - **Root URL** : `https://auth-proxy.homelab.local/oauth2/callback`

3. Clique **Next**

---

## ⚙️ Paramètres importants

Dans l’étape suivante :

- ✅ **Client authentication** : **Activé**
    - Ce paramètre rend le client **confidential**
- ❌ **Authorization** : Désactivé
- ❌ **Service accounts** : Désactivé

Puis clique **Save**

---

## 📥 Configurer les URI de redirection

1. Dans l’onglet **Settings**, descends à la section **Access settings**

2. Ajoute dans **Valid redirect URIs** :

```
https://auth-proxy.homelab.local/oauth2/callback
```

3. Ajoute dans **Web origins** :

```
*
```

Et sauvegarde.

---

## 🔑 Récupérer le Client Secret

1. Va dans l’onglet **Credentials**
2. Copie la valeur du champ **Client secret**

---

## ✅ Résumé pour `.env`

```env
KEYCLOAK_CLIENT_ID=portainer
KEYCLOAK_CLIENT_SECRET=colle-le-ici
```

---

Tu es maintenant prêt à utiliser Keycloak comme fournisseur OIDC pour sécuriser Portainer via `oauth2-proxy`.
