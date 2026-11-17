# Carnet Macros

Suivi des macros de la journée : tu enregistres tes aliments avec leurs valeurs pour 100 g (ou pour 1 pièce), tu choisis tes grammages du jour, l'app calcule les kcal, protéines, glucides, lipides et fibres.

Ce guide se fait en deux parties :

- **Partie 1** (10 min) : mettre l'app en ligne avec GitHub Pages. Elle marche alors sur ton PC et ton téléphone, mais chaque appareil garde ses propres données.
- **Partie 2** (15 min, gratuit) : créer un compte pour que le PC et le téléphone partagent les mêmes données.

Ta liste d'aliments et ta journée en cours (version Claude, 17 novembre 2026) sont déjà chargées dans l'app.

---

## Contenu du dossier

| Fichier | Rôle |
|---|---|
| `index.html` | l'app |
| `config.js` | la configuration de la synchronisation (à remplir à l'étape 5) |
| `firestore.rules` | les règles de sécurité à copier dans Firebase (étape 4) |
| `sw.js`, `manifest.webmanifest`, les `.png` | pour installer l'app sur l'écran d'accueil et l'ouvrir sans réseau |
| `README.md` | ce guide |

---

## Partie 1 — Mettre l'app en ligne (GitHub Pages)

1. Connecte-toi sur **github.com** (crée un compte gratuit si besoin).
2. En haut à droite, **+** puis **New repository**.
   - *Repository name* : `carnet-macros`
   - Visibilité : **Public** (obligatoire pour GitHub Pages avec un compte gratuit)
   - Clique **Create repository**.
3. Sur la page du dépôt vide, clique le lien **uploading an existing file**.
   Dézippe le dossier, sélectionne **tous les fichiers qu'il contient** (pas le dossier lui-même) et glisse-les dans la page. En bas, **Commit changes**.
4. Dans le dépôt : onglet **Settings**, puis **Pages** dans le menu de gauche.
   Sous *Build and deployment* : *Source* = **Deploy from a branch**, *Branch* = **main** et **/ (root)**, puis **Save**.
5. Attends une à deux minutes et recharge la page : l'adresse de ton app s'affiche en haut, du type
   `https://TON-PSEUDO.github.io/carnet-macros/`

Ouvre cette adresse sur ton PC et sur ton téléphone. L'app fonctionne déjà ; pour partager les données entre les deux, passe à la partie 2.

---

## Partie 2 — Compte et synchronisation (Firebase)

Firebase est un service de Google qui va gérer ton compte (pseudo + mot de passe) et stocker tes données. Le plan gratuit (Spark) ne demande pas de carte bancaire, et l'usage d'une seule personne reste très loin de ses limites.

Les noms de boutons ci-dessous peuvent varier légèrement selon la langue de la console (français ou anglais).

### Étape 1 — Créer le projet

1. Va sur **console.firebase.google.com** et connecte-toi avec un compte Google.
2. **Créer un projet** (ou *Ajouter un projet*). Nom : `carnet-macros`.
3. Google Analytics ne sert à rien ici : tu peux le désactiver. Valide jusqu'à ce que le projet soit prêt.

### Étape 2 — Déclarer l'app web

1. Sur la page d'accueil du projet, clique l'icône **`</>`** (Web).
2. Surnom de l'app : `Carnet Macros`. **Ne coche pas** Firebase Hosting. Clique **Enregistrer l'application**.
3. Firebase affiche un bloc de code qui contient `const firebaseConfig = { apiKey: "…", … }`.
   Garde cette page ouverte, tu en as besoin à l'étape 5.
   (Tu la retrouves plus tard dans ⚙ **Paramètres du projet** → **Général** → *Vos applications*.)

### Étape 3 — Activer la connexion par e-mail

1. Menu de gauche : **Authentication** (parfois rangé sous *Créer* / *Build*), puis **Commencer**.
2. Onglet **Mode de connexion** (*Sign-in method*) → **Adresse e-mail/Mot de passe**.
3. Active le **premier** interrupteur (pas besoin de « lien envoyé par e-mail ») → **Enregistrer**.
   Tu te connecteras avec un pseudo, pas avec ton e-mail : l'app transforme le pseudo en adresse interne au projet, qui ne reçoit jamais de courrier. C'est pour cela qu'il faut activer ce mode de connexion.
4. Onglet **Paramètres** (*Settings*) → **Domaines autorisés** → **Ajouter un domaine** :
   `TON-PSEUDO.github.io` (sans `https://` et sans `/carnet-macros`).

### Étape 4 — Créer la base de données et la protéger

1. Menu de gauche : **Firestore Database** → **Créer une base de données**.
   - Si on te demande une édition, garde **Standard**.
   - Emplacement : choisis une région en Europe (par exemple `eur3` ou `europe-west9 (Paris)`). Ce choix ne peut plus être changé ensuite.
   - Mode : **production**. Crée la base.
2. Onglet **Règles** (*Rules*) : efface tout le texte, colle à la place le contenu du fichier `firestore.rules`, puis **Publier**.

Ces règles font que chaque compte ne peut lire et modifier que ses propres données.

### Étape 5 — Coller la configuration dans `config.js`

1. Sur GitHub, dans ton dépôt, clique sur `config.js`, puis sur l'icône **crayon** (*Edit this file*).
2. Remplace chaque `""` par la valeur correspondante du bloc `firebaseConfig` de l'étape 2. Garde les guillemets et les virgules. S'il y a une ligne `measurementId`, tu peux l'ignorer.

   Exemple (valeurs inventées) :

   ```js
   window.CARNET_FIREBASE_CONFIG = {
     apiKey: "AIzaSyB-exemple-exemple-exemple",
     authDomain: "carnet-macros-12345.firebaseapp.com",
     projectId: "carnet-macros-12345",
     storageBucket: "carnet-macros-12345.appspot.com",
     messagingSenderId: "123456789012",
     appId: "1:123456789012:web:abcdef123456"
   };
   ```

3. **Commit changes**. Attends quelques minutes que GitHub Pages se mette à jour, puis recharge l'app.

### Étape 6 — Créer ton compte dans l'app

1. Ouvre l'app **sur l'appareil où ta liste est la plus à jour**, onglet **Compte**.
2. Choisis un pseudo (3 à 30 caractères : lettres, chiffres, `-` ou `_`) et un mot de passe (6 caractères minimum), puis **Créer un compte**.
   Les majuscules et les accents du pseudo ne comptent pas : « Tom » et « tom » sont le même compte.
   Les aliments et la journée de cet appareil partent en ligne. L'indicateur en haut à droite passe à « Synchronisé ».
3. Sur l'autre appareil : même adresse de l'app, onglet **Compte**, même pseudo et même mot de passe, **Se connecter**.
   Les données en ligne remplacent celles de cet appareil.

C'est fini : chaque modification arrive sur l'autre appareil quelques secondes après.

---

## Onglet Objectifs

Renseigne ton profil (sexe, âge, taille, poids, et ta masse grasse si tu la connais), ton niveau d'activité et ton objectif (perte de poids, sèche, maintien, prise de masse). Pour le niveau d'activité, tu peux choisir une catégorie, ou « Détailler mes activités » : sommeil, travail, vie courante, trajets, marche et séances de sport par semaine, d'où l'app calcule ton PAL. L'app :

- calcule ton métabolisme de repos avec plusieurs équations publiées et retient leur médiane ;
- en déduit ta dépense quotidienne, puis ton apport cible selon le rythme visé ;
- répartit cet apport avec des curseurs (protéines en g/kg, lipides en % des calories, glucides en reste), chacun avec sa zone recommandée et sa source ;
- affiche ces objectifs dans l'étiquette de ta journée (objectif, reste, barres de progression) ;
- suit ton poids : pesées datées, courbe avec moyenne sur 7 jours, tendance comparée au rythme visé.

La carte « Sources et limites », en bas de l'onglet, donne les références complètes. Ces données sont synchronisées comme le reste : aucune étape supplémentaire dans Firebase.

---

## Installer l'app sur le téléphone

- **iPhone (Safari)** : bouton Partager, puis « Sur l'écran d'accueil ».
- **Android (Chrome)** : menu ⋮, puis « Installer l'application » ou « Ajouter à l'écran d'accueil ».

L'app s'ouvre alors en plein écran, sans la barre du navigateur.

---

## Bon à savoir

- **Sans réseau**, l'app fonctionne normalement ; les modifications partent quand la connexion revient.
- **L'indicateur en haut à droite** donne l'état : *Synchronisé*, *Synchronisation…*, *Hors ligne*, *Non connecté*, *Erreur de synchro*, ou *Sur cet appareil* (synchro pas encore activée). Touche-le pour voir le détail dans l'onglet Compte.
- **Mot de passe oublié** : avec un pseudo, aucun e-mail de récupération ne peut être envoyé. Laisse ton navigateur ou ton téléphone mémoriser le mot de passe quand il le propose. En dernier recours, tes données restent dans Firebase (Firestore Database → `users`) et je peux t'aider à les récupérer.
  Si tu préfères un compte récupérable, tape une vraie adresse e-mail à la place du pseudo : l'app l'accepte aussi, et le lien « Mot de passe oublié ? » apparaît alors.
- **Sauvegarde** : onglet Compte → *Exporter mes données* télécharge un fichier `.json` ; *Importer un fichier* le restaure.
- **Sécurité** : les valeurs de `config.js` ne sont pas secrètes, Firebase est prévu pour qu'elles soient visibles. Ce sont les règles de l'étape 4 qui protègent tes données. Ton dépôt étant public, la liste d'aliments de départ écrite dans `index.html` est visible par qui ouvre le code ; tes modifications ultérieures, elles, ne vont que dans ta base Firebase.
- **Mettre à jour l'app** : quand tu reçois une nouvelle version, remplace les fichiers sur GitHub (*Add file* → *Upload files*), **sauf `config.js`** qui contient ta configuration. Si tu l'écrases par erreur, refais l'étape 5.

---

## Si quelque chose bloque

| Message dans l'onglet Compte | Quoi faire |
|---|---|
| « La connexion par e-mail n'est pas activée » ou « Authentication n'est pas encore activé » | Étape 3 |
| « Adresse du site non autorisée » | Étape 3, point 4 (domaine `TON-PSEUDO.github.io`) |
| « Firestore refuse l'accès » | Étape 4, point 2 (règles collées puis **Publier**) |
| « La base Firestore n'existe pas encore » | Étape 4, point 1 |
| « La configuration collée dans config.js semble incorrecte » | Étape 5 : vérifie guillemets, virgules et valeurs |
| « Module de synchronisation indisponible » | Pas de réseau au premier lancement : recharge la page une fois en ligne |
| L'onglet Compte parle encore du guide alors que `config.js` est rempli | Attends quelques minutes puis recharge la page (GitHub Pages et le navigateur gardent l'ancienne version un moment) |
