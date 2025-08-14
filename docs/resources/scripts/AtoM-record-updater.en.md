# Metadata injection in AtoM

Ce guide explique comment utiliser le script [`Celui_qui_modifie_les_notices_AtoM.py`](../../resources/scripts/Celui_qui_modifie_les_notices_AtoM.py) afin d’automatiser l’édition des notices dans AtoM, en injectant les métadonnées générées depuis le fichier `METS` d’un DIP Archivematica.

Ce script constitue la deuxième étape du processus après génération du fichier `urls.csv` à l’aide du [script précédent](../assemble-mets-atom/).

---

## Objectif

Une fois le fichier `urls.csv` généré, ce script :

- **se connecte à AtoM** à l’aide d’un compte utilisateur
- **ouvre chaque notice AtoM** indiquée dans `urls.csv`
- **renseigne automatiquement** les champs ISAD-G avec les métadonnées extraites du fichier METS :
  - Titre
  - Date de création
  - Niveau de description
  - Étendue et support
  - Localisation des originaux
  - Cote / Identifiant
  - Événement de création

---

## Prérequis

- Le fichier `urls.csv` généré par le premier script
- Un fichier `login.csv` contenant les identifiants pour se connecter à l’interface d’AtoM
- Le script [`Celui_qui_modifie_les_notices_AtoM.py`](../../resources/scripts/Celui_qui_modifie_les_notices_AtoM.py)
- Python installé sur votre machine
- Une connexion Internet active

!!! warning "Connexion requise"
    Le script interagit avec l’interface web d’AtoM. Il simule une navigation utilisateur, ce qui nécessite une connexion stable et un accès au site en ligne.

### Format attendu de `login.csv`

Le fichier `login.csv` doit se trouver au même niveau que le script. Il contient deux colonnes : `username` et `password`. Exemple :

```csv title="login.csv"
username,password
mon_utilisateur,motdepasse
```

---

## Utilisation

Dans le terminal, placez-vous dans le dossier contenant :

    urls.csv

    login.csv

    Celui_qui_modifie_les_notices_AtoM.py

Puis exécutez :

```bash
python Celui_qui_modifie_les_notices_AtoM.py
```

Le script devrait se connecter automatiquement à AtoM, parcourir les notices mentionnées dans urls.csv, et injecter les métadonnées ligne par ligne.

---

## Fonctionnement technique

Le script repose probablement sur une bibliothèque d’automatisation (ex. Selenium ou requests + BeautifulSoup) pour :

- Accéder à l’interface de connexion d’AtoM
- Naviguer jusqu’à chaque notice (via les URLs FR et EN du CSV)
- Remplir les champs appropriés
- Enregistrer les modifications

## À savoir

- Le processus peut prendre un certain temps en fonction du nombre de notices à modifier.
- Aucune sauvegarde automatique n’est effectuée : il est recommandé de tester sur un environnement de développement ou une copie du dépôt AtoM avant un déploiement en production.
- Si le format du fichier urls.csv est incorrect ou incomplet, le script peut échouer silencieusement ou provoquer des erreurs.

## Bonnes pratiques

- Testez d’abord avec 1 ou 2 notices pour valider que tout fonctionne.
- Vérifiez que les champs ISAD-G dans AtoM sont bien configurés pour recevoir les métadonnées attendues.
- Faites une sauvegarde de votre base de données AtoM avant d’exécuter le script à grande échelle.