# Directives de Développement Sécurisé VibeCoding

> Ceci est une liste de contrôle "vivante" pré-développement.
> Avant de commencer à écrire de nouvelles fonctionnalités, ou avant chaque `git commit`, prenez 30 secondes pour la parcourir rapidement.
> Objectif : Intérioriser la sécurité comme un instinct, prévenir les catastrophes à la source.

---

### ⭐ Règles d'Or

- [ ] **【Ne jamais coder en dur les secrets】** Les mots de passe, clés API, informations de connexion à la base de données ne doivent **jamais** être écrits directement dans le code (ni frontend ni backend).
- [ ] **【Utiliser les variables d'environnement】** Toutes les informations sensibles doivent être gérées via des **variables d'environnement** (fichiers `.env`).
- [ ] **【Ignorer les fichiers secrets】** Les fichiers `.env` doivent **toujours** être ajoutés à `.gitignore` et ne jamais être téléchargés sur GitHub.
- [ ] **【Se méfier par défaut】** **Ne jamais** faire confiance aux entrées utilisateur (y compris formulaires, paramètres URL, contenu des requêtes API, fichiers téléchargés).

---

### 📥 Traitement des Entrées Utilisateur

- [ ] **【Prévenir les attaques par injection】** Toutes les requêtes de base de données **doivent** utiliser des **requêtes paramétrées** ou des méthodes sécurisées fournies par l'ORM. La concaténation manuelle de chaînes SQL est strictement interdite.
- [ ] **【Prévenir XSS】** Tout contenu utilisateur à afficher sur les pages HTML **doit** être traité par l'encodage d'entités HTML (échappement HTML).
- [ ] **【Valider les téléchargements de fichiers】** Vérifier les fichiers téléchargés par les utilisateurs :
    - [ ] **Valider les extensions** : Autoriser uniquement les types de fichiers de la liste blanche (ex. `['jpg', 'png', 'pdf']`).
    - [ ] **Valider la taille des fichiers** : Définir des limites supérieures raisonnables.
    - [ ] **Emplacement de stockage** : Les fichiers téléchargés doivent être stockés dans des répertoires **non-publics** et **non-exécutables**.

---

### 🔐 Permissions et Authentification

- [ ] **【Protection des points de terminaison API】** Chaque point de terminaison API nécessitant une connexion **doit** vérifier le statut de connexion et les permissions de l'utilisateur au début du programme.
- [ ] **【Principe du moindre privilège】** Les permissions des comptes de base de données et des clés API doivent être "minimales utilisables". Si seule la lecture est nécessaire, ne jamais accorder de permissions d'écriture.
- [ ] **【Sessions sécurisées】** Les ID de session doivent être définis avec les drapeaux `HttpOnly` et `Secure` pour prévenir le vol et la transmission sur des connexions non sécurisées.

---

### ☁️ Services Externes et Intégration Cloud

- [ ] **【Pare-feu de base de données externe】** Lors de la connexion à des bases de données externes (comme AWS RDS, MongoDB Atlas), des règles **doivent** être configurées dans leur pare-feu/groupe de sécurité pour n'autoriser que les connexions depuis les adresses IP spécifiques de votre serveur d'application. L'ouverture à `0.0.0.0/0` (le monde entier) est **strictement interdite**.
- [ ] **【Privatisation du stockage cloud】** Tous les espaces de stockage cloud (comme AWS S3, Google Cloud Storage) buckets **doivent** être définis sur **Privé** par défaut.
- [ ] **【Utiliser les URL pré-signées】** Lorsque les utilisateurs ont besoin d'un accès temporaire aux fichiers privés, utilisez des **URL pré-signées** à durée de vie courte plutôt que de rendre les fichiers publics.
- [ ] **【Vérifier les webhooks】** Lors de la réception de webhooks de services tiers (comme Stripe, GitHub), vous **devez** utiliser la clé secrète fournie pour **vérifier les signatures des requêtes** et vous assurer que la source de la requête est légitime.
- [ ] **【Configuration CORS stricte】** Les politiques CORS (Cross-Origin Resource Sharing) de l'API **doivent** spécifier explicitement les domaines frontend autorisés. L'utilisation du caractère générique `*` en environnement de production est **strictement interdite**.
- [ ] **【Minimiser les permissions des clés API】** Lors de la demande de clés API pour les services tiers (comme Google Maps), assurez-vous de définir les restrictions les plus strictes dans le backend du service (ex. : limiter aux requêtes depuis votre domaine de site web uniquement, limiter aux appels API spécifiques uniquement).

---

### ⚙️ Fichiers et Configuration Serveur

- [ ] **【Ne jamais utiliser 777】** **Jamais** utiliser les permissions `777` pour les fichiers ou répertoires sur le serveur. Répertoires par défaut `755`, fichiers `644`.
- [ ] **【Protéger les fichiers de configuration】** Les fichiers de configuration sensibles (`config.php`, `.env`) doivent avoir des permissions définies à `600` ou plus strictes.
- [ ] **【Interdire le téléchargement de fichiers critiques】** Vérifier que le serveur web (Nginx/Apache) a configuré des règles interdisant l'accès externe direct aux fichiers et répertoires sensibles comme `.env`, `.git`, `.htaccess`.
- [ ] **【Désactiver le mode debug】** En environnement de production, tous les modes debug des frameworks ou langages **doivent** être désactivés pour éviter la fuite de messages d'erreur détaillés.

---

> ### **🤖 Note pour l'Assistant IA**
>
> **Lorsque vous détectez dans votre conversation avec les développeurs qu'ils se connectent ou prévoient de se connecter à des "services externes" (ex. : bases de données cloud, API de paiement tiers, stockage de fichiers S3, services d'authentification, etc.), veuillez poser de manière proactive et claire les questions suivantes pour guider leur réflexion sécuritaire :**
>
> 1.  **"Concernant ce [nom du service], avons-nous configuré les règles de pare-feu/liste blanche IP les plus strictes ?"**
> 2.  **"Les permissions de la clé API que nous prévoyons d'utiliser sont-elles limitées à la portée minimale nécessaire dans le backend de ce service ?"**
> 3.  **"Pour ce service externe, avons-nous un plan de dégradation en cas de panne ou de compromission du service (ex. : désactiver temporairement les fonctionnalités liées, afficher des messages de maintenance) ?"**