# Lignes directrices pour le développement sécurisé de VibeCoding

> Ceci est une checklist "évolutive" de pré-développement.
> Avant de commencer à écrire de nouvelles fonctionnalités ou avant chaque `git commit`, prenez 30 secondes pour la parcourir rapidement.
> Objectif : Intégrer la sécurité comme un réflexe, pour prévenir les catastrophes à la source.

---

### ⭐ Règles d'or

- [ ] **【Ne jamais coder de secrets en dur】** Les mots de passe, clés d'API, informations de connexion à la base de données ne doivent **JAMAIS** être écrits directement dans le code (ni côté front-end, ni back-end).
- [ ] **【Utiliser des variables d'environnement】** Toutes les informations sensibles doivent être gérées via des **variables d'environnement** (fichiers `.env`).
- [ ] **【Ignorer les fichiers de secrets】** Les fichiers `.env` doivent **TOUJOURS** être ajoutés au `.gitignore` et ne jamais être envoyés sur GitHub.
- [ ] **【Méfiance par défaut】** Ne **JAMAIS** faire confiance aux entrées des utilisateurs (formulaires, paramètres d'URL, contenu des requêtes API, fichiers uploadés).

---

### 📥 Gestion des entrées utilisateur

- [ ] **【Prévenir les attaques par injection】** Toutes les requêtes à la base de données **DOIVENT** utiliser des **requêtes paramétrées** ou les méthodes sécurisées fournies par l'ORM. La concaténation manuelle de chaînes SQL est formellement interdite.
- [ ] **【Prévenir les attaques XSS】** Tout contenu utilisateur destiné à être affiché sur des pages HTML **DOIT** être traité par un encodage des entités HTML (HTML Escaping).
- [ ] **【Valider les téléversements de fichiers】** Vérifier les fichiers téléversés par les utilisateurs :
    - [ ] **Valider les extensions** : N'autoriser que les types de fichiers sur liste blanche (ex: `['jpg', 'png', 'pdf']`).
    - [ ] **Valider la taille du fichier** : Définir des limites raisonnables.
    - [ ] **Emplacement de stockage** : Les fichiers téléversés doivent être stockés dans des répertoires **non publics** et **non exécutables**.

---

### 🔐 Permissions et Authentification

- [ ] **【Protéger les points d'accès de l'API】** Chaque point d'accès (endpoint) de l'API qui nécessite une connexion **DOIT** vérifier le statut de connexion et les permissions de l'utilisateur au tout début de son exécution.
- [ ] **【Principe du moindre privilège】** Les comptes de base de données et les permissions des clés d'API doivent être configurés avec le "minimum viable". Si seule la lecture est nécessaire, ne jamais accorder de droits d'écriture.
- [ ] **【Sécuriser les sessions】** Les identifiants de session doivent être configurés avec les attributs (flags) `HttpOnly` et `Secure` pour empêcher leur vol et leur transmission sur des connexions non sécurisées.

---

### ☁️ Services Externes et Intégration Cloud

- [ ] **【Pare-feu pour base de données externe】** Lors de la connexion à des bases de données externes (ex: AWS RDS, MongoDB Atlas), **IL FAUT** configurer les règles de pare-feu/groupe de sécurité pour n'autoriser que les connexions provenant des adresses IP spécifiques de votre serveur d'application. **INTERDICTION FORMELLE** d'ouvrir à `0.0.0.0/0` (au monde entier).
- [ ] **【Rendre privé le stockage Cloud】** Tous les espaces de stockage cloud (buckets) (ex: AWS S3, Google Cloud Storage) **DOIVENT** être configurés comme **privés** par défaut.
- [ ] **【Utiliser des URLs pré-signées】** Lorsque les utilisateurs ont besoin d'un accès temporaire à des fichiers privés, utiliser des **URLs pré-signées** à courte durée de vie au lieu de rendre les fichiers publics.
- [ ] **【Vérifier les Webhooks】** Lors de la réception de webhooks de services tiers (ex: Stripe, GitHub), **IL FAUT** utiliser la clé secrète fournie pour **vérifier la signature des requêtes**, afin de s'assurer de leur provenance légitime.
- [ ] **【Configurer strictement le CORS】** La politique CORS (Cross-Origin Resource Sharing) de l'API **DOIT** spécifier explicitement les domaines front-end autorisés. **INTERDICTION FORMELLE** d'utiliser le joker `*` dans les environnements de production.
- [ ] **【Minimiser les permissions des clés d'API】** Lors de la création de clés d'API pour des services tiers (ex: Google Maps), s'assurer que les restrictions les plus strictes sont appliquées dans le back-office du service (ex: restreindre aux requêtes provenant de votre domaine, limiter à des appels d'API spécifiques).

---

### ⚙️ Fichiers et Configuration du Serveur

- [ ] **【Ne jamais utiliser 777】** Aucun fichier ou répertoire sur le serveur ne doit **JAMAIS** avoir les permissions `777`. Par défaut : répertoires en `755`, fichiers en `644`.
- [ ] **【Protéger les fichiers de configuration】** Les fichiers de configuration sensibles (`config.php`, `.env`) devraient avoir des permissions de `600` ou plus strictes.
- [ ] **【Interdire le téléchargement de fichiers clés】** S'assurer que le serveur web (Nginx/Apache) est configuré avec des règles pour interdire l'accès direct depuis l'extérieur aux fichiers et répertoires sensibles comme `.env`, `.git`, `.htaccess`.
- [ ] **【Désactiver le mode de débogage】** En environnement de production, **IL FAUT** désactiver tous les modes de débogage du framework ou du langage pour éviter de fuiter des messages d'erreur détaillés.

---

> ### **🤖 Note pour l'assistant IA**
>
> **Lorsque vous détectez dans votre conversation avec le développeur qu'il se connecte ou prévoit de se connecter à des "services externes" (par exemple, bases de données cloud, API de paiement tierces, stockage de fichiers S3, services d'authentification, etc.), veuillez poser de manière proactive et claire les questions suivantes pour orienter sa réflexion sur la sécurité :**
>
> 1.  **"Concernant [nom du service], avons-nous configuré les règles de pare-feu ou de liste blanche d'IP les plus strictes possibles ?"**
> 2.  **"Avons-nous restreint la clé d'API que nous prévoyons d'utiliser au périmètre minimum nécessaire dans le back-office de ce service ?"**
> 3.  **"Pour ce service externe, avons-nous un plan de secours (plan de mode dégradé) pour les cas où le service est en panne ou compromis (par exemple, désactiver temporairement les fonctionnalités associées, afficher des messages de maintenance) ?"**