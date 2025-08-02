# Directives de Sécurité VibeCoding Expliquées : Pourquoi Insistons-nous Autant ?

Ce document explique l'importance de chaque règle dans les "Directives de Sécurité". Comprendre cela vous aidera à voir votre code du "point de vue d'un hacker" et à prévenir les problèmes avant qu'ils ne surviennent.

---

## ⭐ Règles d'Or

### 【Ne jamais coder en dur les secrets】 & 【Utiliser les variables d'environnement】 & 【Ignorer les fichiers secrets】

**Pourquoi est-ce important ?**

Ces trois règles sont les lois de survie suprêmes du monde numérique. Votre code (surtout lors de l'utilisation de Git) sera copié, partagé et stocké à plusieurs endroits. Écrire des mots de passe, clés API et autres "secrets" directement dans le code, c'est comme tatouer le mot de passe de votre coffre-fort sur votre front - quiconque vous voit (voit le code) peut facilement ouvrir votre coffre-fort.

**Scénario du Hacker 😈**
> J'adore chercher `password`, `api_key` ou `db_connect` sur GitHub. J'ai trouvé votre projet public et dans un fichier appelé `config.js`, j'ai vu ce code : `const db_password = 'Password123!';`. Parfait ! Je n'ai même pas besoin d'attaquer votre site - je peux maintenant essayer directement de me connecter à votre base de données avec ce mot de passe.

**Conséquences Catastrophiques 💥**

**Prise de contrôle complète du service.** Les hackers peuvent voler, altérer, supprimer toutes vos données utilisateur, ou utiliser vos services API payants pour des activités illégales, toutes les factures étant à votre charge.

---

## 📥 Traitement des Entrées Utilisateur

### 【Prévenir les Attaques par Injection (SQL Injection)】

**Pourquoi est-ce important ?**

Imaginez la base de données comme un robot qui ne comprend que le langage SQL. Si vous concaténez directement les entrées utilisateur avec vos commandes, les utilisateurs ont l'opportunité de prononcer leurs propres "commandes". Les requêtes paramétrées disent au robot : "Écoute, la partie suivante est **juste des données** - peu importe le contenu, ne l'exécute pas comme des commandes."

**Scénario du Hacker 😈**
> Dans la boîte de connexion de votre site, j'ai entré `' OR '1'='1' --` comme nom d'utilisateur. Je suppose que votre requête SQL est écrite ainsi : `"SELECT * FROM users WHERE username = '" + userInput + "';"`. Mon entrée l'a transformée en `SELECT * FROM users WHERE username = '' OR '1'='1' --';`. `'1'='1'` est toujours vrai, donc j'ai contourné la vérification du mot de passe et me suis connecté avec succès au premier compte utilisateur (généralement l'administrateur).

**Conséquences Catastrophiques 💥**

Les attaquants peuvent contourner la connexion, voler toutes les données de la base de données (listes d'utilisateurs, hashes de mots de passe), voire supprimer la base de données.

---

## 🔐 Permissions et Authentification

### 【Protection des Points de Terminaison API】

**Pourquoi est-ce important ?**

Les interfaces frontend (UI) peuvent cacher des boutons, mais les hackers ne se fient jamais aux interfaces. Ils appellent directement vos API backend avec des outils (comme Postman, curl). Vous devez supposer que chaque point de terminaison API sera attaqué directement, donc chaque point de terminaison doit être un garde indépendant, vérifiant lui-même l'identité et les permissions des visiteurs.

**Scénario du Hacker 😈**
> J'ai découvert que pour modifier les informations personnelles, le frontend envoie une requête à `/api/user/update`. Bien que je ne puisse pas voir les boutons de modification d'autres personnes, je suppose que cette API distingue les utilisateurs via `userId`. J'ai essayé d'envoyer une requête à `/api/user/update?userId=1` (ID de l'administrateur) avec les données que je voulais modifier. Mon Dieu, le serveur n'a pas vérifié si la requête venait de moi personnellement et a réussi à changer l'email de l'administrateur !

**Conséquences Catastrophiques 💥**

Les utilisateurs ordinaires peuvent altérer les données d'autres utilisateurs ou même des administrateurs, ou exécuter des permissions qu'ils ne devraient pas avoir, causant un chaos système.