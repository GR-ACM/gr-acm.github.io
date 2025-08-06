# Configuration avancée

---

## Support bilingue

Le wiki des Archives de la construction moderne (Acm) utilise un système de documentation bilingue français/anglais, mis en œuvre grâce au plugin `mkdocs-static-i18n` pour MkDocs. Ce plugin permet de gérer plusieurs langues au sein d’un même dépôt, tout en conservant une structure de navigation adaptée à chaque langue. [Consultez la documentation officielle ici](https://ultrabug.github.io/mkdocs-static-i18n/getting-started/quick-start/).

### Installation du plugin

Dans l’environnement Python utilisé pour générer le site, le plugin `mkdocs-static-i18n` doit être installé :

```bash
pip install mkdocs-static-i18n
```

Puis activé dans le fichier `mkdocs.yml`:

```yaml
features:
  - i18n

plugins:
  - i18n
```

### Structure et navigation

```psql
docs/
├── index.fr.md
├── index.en.md
└── wiki/
    ├── overview.fr.md
    ├── overview.en.md
    ├── installation.fr.md
    └── installation.en.md
```

Dans cette configuration, les fichiers sont nommés avec des suffixes de langue (`.fr.md`, `.en.md`, etc.) et placés directement dans le dossier `docs/`. Le plugin identifie les fichiers en fonction de leur suffixe de langue et il n'est pas nécessaire d'inclure le suffixe dans `nav:`.

La barre de recherche peut disparaître, le plugin search doit être déclaré explicitement.

```yaml
plugins:
  - search
  - privacy
  - i18n:
      languages:
        - locale: fr
          name: Français
          build: true
          default: true
          nav:
            - Accueil: index.md
            - À propos:
                - Présentation technique: wiki/overview.md
                - Installation initiale: wiki/installation.md

        - locale: en
          name: English
          build: true
          nav:
            - Home: index.md
            - About:
                - Technical overview: wiki/overview.md
                - Installation: wiki/installation.md
```

Dans cet exemple, le français est défini comme langue par défaut.  
L’URL racine du site, `https://mon-site/`, affiche automatiquement la version française.
Cette URL correspond au fichier `docs/index.fr.md`, qui devient la page d’accueil.  
L’URL `https://mon-site/en/` affiche la version anglaise du site.
Cette URL correspond au fichier `docs/index.en.md`.

| URL                    | Fichier source utilisé | Langue                      |
| ---------------------- | ---------------------- | --------------------------- |
| `https://mon-site/`    | `docs/index.fr.md`     | :flag_fr: Français (défaut) |
| `https://mon-site/en/` | `docs/index.en.md`     | :flag_gb: Anglais           |
