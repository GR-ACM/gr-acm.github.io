# Installation

Cette documentation décrit la manière dont ce wiki a été déployé. Elle a pour objectif de documenter les étapes techniques suivies lors de sa création, tout en fournissant un guide reproductible pour celles et ceux qui souhaiteraient construire un site similaire.

Ce guide s’appuie directement sur la [documentation officielle de Material for MkDocs](https://squidfunk.github.io/mkdocs-material/getting-started/), dont il constitue une version condensée, simplifiée et adaptée au contexte de ce projet.

---

## Prérequis

Avant de commencer l’installation, assurez-vous de disposer des outils suivants :

- [:simple-python: Python](https://www.python.org/downloads/), un langage de programmation simple, lisible et largement utilisé. Nous recommandons la version 3.4 ou plus récente pour suivre ce guide.

    - [:material-language-python: pip](https://pip.pypa.io/en/stable/installation/), le gestionnaire de paquets de Python. Il permet d’installer des bibliothèques et des outils. Il est inclus par défaut avec Python à partir de la version 3.4.

- :fontawesome-solid-file-code: Un éditeur de code. Nous recommandons [:simple-vscodium: VSCodium](https://vscodium.com/), une alternative open source à [:material-microsoft-visual-studio-code: Visual Studio Code](https://code.visualstudio.com/).

- [:simple-github: Un compte GitHub](http://github.com/), puisque nous allons déployer le site via le service [GitHub Pages](https://pages.github.com/).

!!! tip "Conseil"
    Une fois installé, vous pouvez vérifier que Python fonctionne en ouvrant un terminal et en tapant :

        python --version
        
    Pour vérifier que `pip` fonctionne :

        pip --version

---

## Installation initiale

Ouvrez un terminal dans le dossier où vous souhaitez créer le projet :

=== ":material-microsoft-windows: Windows"
    Ouvrez :material-powershell: Powershell

=== ":material-apple: macOS"
    Ouvrez :material-console: Terminal

Le terminal exécute les commandes dans un dossier précis. Il est important de vous placer dans le dossier où vous souhaitez créer votre wiki.
La commande `cd` (“change directory”) permet de naviguer dans l’arborescence des dossiers depuis le terminal.

```bash
cd chemin/vers/mon/dossier
```

!!! tip "Exécuter les commandes depuis l'éditeur"
    Visual Studio Code et VSCodium disposent d'un terminal intégré, qui vous permet de continuer à taper vos commandes directement depuis l’éditeur. Allez dans le menu **Terminal** > **Nouveau terminal**, ou utilisez le raccourci.

### Créer un environnement virtuel Python

!!! note inline end "Que signifie cette commande ?"
    `-m venv` utilise le module intégré venv, qui sert à créer des environnements virtuels.
    `venv` est le nom du dossier qui sera créé dans votre projet (vous pouvez choisir un autre nom, mais `venv` est une convention courante).

Dans le terminal, tapez la commande suivante :

```bash
python -m venv venv
```

### Activer l'environnement virtuel

=== ":material-microsoft-windows: Windows"
    Tapez `.\venv\Scripts\activate`

=== ":material-apple: macOS"
    Tapez `source venv/bin/activate`

!!! success "Activation"
    Si tout fonctionne correctement, vous verrez le nom de l’environnement `(venv)` apparaître au début de votre ligne de commande.

À chaque fois que vous fermez le terminal, vous devrez réactiver l’environnement la prochaine fois, avec la même commande que ci-dessus.

### Installer Material for MkDocs avec `pip`

```bash
pip install mkdocs-material
```

Cette commande installe à la fois MkDocs, Material for MkDocs, et toutes les dépendances nécessaires.

Après l’installation, vous pouvez vérifier que MkDocs est bien disponible en tapant `mkdocs --version`.

### Ouvrir le dossier du projet dans Visual Studio Code ou VSCodium

!!! note inline end "Que signifie cette commande ?"
    `code` appelle l’application Visual Studio Code / VSCodium, 
    `.` signifie “le dossier courant”.

Vous pouvez utiliser la commande suivante dans votre terminal (dans le bon dossier) :

```bash
code .
```

### Initialiser un nouveau site MkDocs

Dans le terminal (toujours avec l’environnement virtuel activé), tapez la commande suivante :

```bash
mkdocs new .
```

La structure suivante devrait avoir été créée :

```pgsql
.
├─ docs/
│  └─ index.md
└─ mkdocs.yml
```

Cette commande crée deux éléments importants :

- `docs/index.md` : la première page du site (accueil). Écrite en Markdown, elle peut être modifiée librement.
- `mkdocs.yml` : un fichier de configuration rédigé en YAML (titre, thème, navigation, langues, etc.).

Pour ajouter une nouvelle page à votre site, il suffit de créer un fichier `.md` dans le dossier `docs/`.
Par exemple, si vous créez un fichier `apropos.md`, il sera accessible à l’adresse https://mon-site/apropos/.

!!! note "Besoin d’un rappel sur la syntaxe Markdown ?"
    Consultez notre [guide de syntaxe Markdown](../structure/#markdown) pour les bases.

### Configurer le site avec `mkdocs.yml` 

Ouvrez `mkdocs.yml` dans votre éditeur et remplacez son contenu par la configuration suivante :

```yaml title="mkdocs.yml"
site_name: Documentation de mon projet
site_url: https://mon-site.exemple
theme:
  name: material
```

## Prévisualiser le site en local

Maintenant que votre site est configuré, vous pouvez le générer et le visualiser localement en temps réel pendant que vous travaillez dessus.

Dans le terminal (avec l’environnement virtuel activé), tapez :

```bash
mkdocs serve
```

Ouvrez un navigateur à l'adresse : [http://localhost:8000/](http://localhost:8000/)

!!! tip "Visualisez vos modifications en temps réel"
    Tant que la commande `mkdocs serve` est active dans votre terminal, le site reste visible et se met à jour automatiquement à chaque sauvegarde.

Quand vous avez terminé, vous pouvez arrêter le serveur en appuyant dans le terminal :

=== ":material-microsoft-windows: Windows"
    Appuyer simultanément sur ++ctrl+c++

=== ":material-apple: macOS"
    Appuyer simultanément sur ++cmd+c++

## Validation automatique du schéma YAML

!!! tip ""
    Le fichier `mkdocs.yml` est central dans la configuration de votre site. Pour éviter les erreurs de syntaxe ou de structure (qui peuvent bloquer la génération du site), il est fortement recommandé d’activer la validation automatique du schéma YAML dans votre éditeur de code.

Pour Visual Studio Code et VSCodium, recherchez *YAML* dans le panneau des extensions et installez l'extension officielle YAML par Red Hat.

Recherchez le fichier `settings.json` dans les paramètres ou créez-en un autre à la racine du projet.

Ajoutez le code suivant dans ce fichier :

```json title="settings.json"
{
  "yaml.schemas": {
    "https://squidfunk.github.io/mkdocs-material/schema.json": "mkdocs.yml"
  },
  "yaml.customTags": [ 
    "!ENV scalar",
    "!ENV sequence",
    "!relative scalar",
    "tag:yaml.org,2002:python/name:material.extensions.emoji.to_svg",
    "tag:yaml.org,2002:python/name:material.extensions.emoji.twemoji",
    "tag:yaml.org,2002:python/name:pymdownx.superfences.fence_code_format",
    "tag:yaml.org,2002:python/object/apply:pymdownx.slugs.slugify mapping"
  ]
}
```
## Publication

Après avoir créé et personnalisé votre site en local, vous pouvez le mettre en ligne gratuitement avec GitHub Pages. Consultez la [section publication](../publish) pour savoir comment publier votre site pour la première fois, puis comment le mettre à jour facilement par la suite.
