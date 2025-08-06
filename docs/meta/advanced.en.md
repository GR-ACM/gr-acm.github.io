# Advanced configuration

---

## Bilingual support

The wiki of the Archives de la construction moderne (Acm) uses a bilingual French/English documentation system, implemented using the `mkdocs-static-i18n` plugin for MkDocs. This plugin allows you to manage multiple languages within a single repository while maintaining a dedicated navigation structure for each language. [View the official documentation here](https://ultrabug.github.io/mkdocs-static-i18n/getting-started/quick-start/).

### Plugin installation

In the Python environment used to build the site, the `mkdocs-static-i18n` plugin must be installed:

```bash
pip install mkdocs-static-i18n
```

Then activated in the `mkdocs.yml` file:

```yaml
features:
  - i18n

plugins:
  - i18n
```

### Structure and navigation

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

In this configuration, files are named with language suffixes (`.fr.md`, `.en.md`, etc.) and placed directly in the `docs/` folder. The plugin detects the files based on their language suffix, and there's no need to include the suffix in `nav:`.

The search bar may disappear, the search plugin must be explicitly declared.

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

In this example, French is defined as the default language.  
The root URL of the site, `https://my-site/`, automatically displays the French version.
This URL corresponds to the `docs/index.fr.md` file, which becomes the homepage.  
The URL `https://my-site/en/` displays the English version of the site.
This URL corresponds to the docs/index.en.md file.

 URL                    | Source file used   | Language                    |
| --------------------- | ------------------ | --------------------------- |
| `https://my-site/`    | `docs/index.fr.md` | :flag_fr: French (default)  |
| `https://my-site/en/` | `docs/index.en.md` | :flag_gb: English           |
