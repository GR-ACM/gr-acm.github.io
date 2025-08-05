# Personnalisation visuelle

Cette section présente différentes façons de personnaliser l’apparence et le comportement de votre site MkDocs, en s’appuyant sur les nombreuses fonctionnalités offertes par **Material for MkDocs**.

---

## Changer l'apparence du site

Material for MkDocs permet de définir des jeux de couleurs personnalisés et de proposer un bouton permettant aux utilisateurs de passer du mode clair au mode sombre.

### Sélectionner le thème

Material for MkDocs dispose de deux thèmes: un mode clair `default` et un mode sombre `slate`.

Dans le fichier `mkdocs.yml`, ajoutez ou modifiez la section `theme:` comme suit :

```yaml
theme:
  name: material
  palette:
    scheme: slate
```

### Définir les couleurs

Les couleurs principales du thème sont définies par `primary` et `accent`.

`primary` définit la couleur dominante du thème. Elle est utilisée notamment pour la barre de navigation supérieure, les liens actifs et certains boutons ou composants interactifs.

```yaml
theme:
  name: material
  palette:
    primary: deep purple
```

`accent` définit la couleur secondaire. Elle est utilisée pour attirer l’attention sur certains éléments, comme les liens survolés, les icônes sélectionnées et certains effets visuels (surbrillance, indicateurs de sélection, etc.).

```yaml
theme:
  name: material
  palette:
    accent: pink
```

Vous pouvez choisir parmi une série de couleurs prédéfinies proposées par le thème, comme `blue`, `cyan`, `green`, `teal`, `deep orange`, etc.
[Consultez la liste complète ici](https://squidfunk.github.io/mkdocs-material/setup/changing-the-colors/#primary-color).

Il est également possible d’utiliser des couleurs personnalisées, en définissant votre propre palette (par code hexadécimal ou variable CSS).
[Voir la documentation officielle](https://squidfunk.github.io/mkdocs-material/setup/changing-the-colors/#custom-colors).

### Activer le mode clair / sombre

Proposer une palette de couleurs claire et sombre rend la documentation plus agréable à lire à tout moment de la journée, en laissant à l’utilisateur le choix du mode qui lui convient le mieux.

La configuration suivante ajoutera un bouton de bascule à gauche de la barre de recherche pour passer d’un thème à l’autre.

!!! tip "Personnalisez les modes clair / sombre"
    Vous pouvez également définir des couleurs différentes pour `primary` et `accent` selon la palette utilisée (claire ou sombre).

```yaml
theme:
  name: material

  palette:
    - scheme: slate
      toggle:
        icon: material/weather-sunny
        name: Passer en mode clair

    - scheme: default
      toggle:
        icon: material/weather-night
        name: Passer en mode sombre
```

Le bouton utilise des icônes Material Design, que vous pouvez personnaliser avec la propriété `toggle:icon`.

Par défaut, on utilise :

- `material/weather-night` :material-weather-night: pour indiquer le passage en mode sombre ;
- `material/weather-sunny` :material-weather-sunny: pour revenir au mode clair.

Vous pouvez remplacer ces icônes par n’importe quelle autre issue de la bibliothèque Material Design Icons.
[Voir la liste complète des icônes disponibles ici](https://squidfunk.github.io/mkdocs-material/reference/icons-emojis/).

---

## Changer la police d'écriture

Material for MkDocs facilite le changement des polices du site, grâce à une intégration directe avec [Google Fonts](https://fonts.google.com/). Vous pouvez personnaliser à la fois la police régulière (texte courant) et la police à chasse fixe (pour les blocs de code) ou même [charger des polices personnalisées](https://squidfunk.github.io/mkdocs-material/setup/changing-the-fonts/#additional-fonts) si besoin.

```yaml
theme:
  name: material
  font:
    text: Roboto
    code: Roboto Mono
```

### Utiliser des icônes et emojis

Material for MkDocs permet d’intégrer facilement des icônes et emojis directement dans le contenu de vos pages Markdown, grâce à l’extension `pymdownx.emoji`. Pour cela, ajoutez la configuration suivante dans la section `markdown_extensions` de votre fichier `mkdocs.yml` :

```yaml
markdown_extensions:
  - attr_list
  - pymdownx.emoji:
      emoji_index: !!python/name:material.extensions.emoji.twemoji
      emoji_generator: !!python/name:material.extensions.emoji.to_svg
```

Cette configuration permet d’utiliser des emojis dans vos textes et ils seront rendus comme des icônes vectorielles stylisées, compatibles avec le thème et adaptables au mode clair/sombre.
[Voir la liste complète des icônes et emojis disponibles ici](https://squidfunk.github.io/mkdocs-material/reference/icons-emojis/).

!!! warning ""L'éditeur me signale une erreur de syntaxe!""
    C'est normal. La syntaxe `!!python/name:...` n'est pas standard YAML et n’est comprise que par PyYAML (le parseur utilisé par MkDocs). Vous pouvez ignorer cette erreur.

### Protection des données

L'utilisation de Google Fonts implique que le navigateur de l’utilisateur envoie une requête à un serveur Google pour charger les polices. Cette pratique peut poser problème du point de vue de la protection des données personnelles.

En Europe, le Règlement général sur la protection des données (RGPD) stipule que l’adresse IP d’un utilisateur est considérée comme une donnée personnelle. Or, le chargement de Google Fonts depuis un serveur tiers (comme Google) transmet cette IP sans consentement explicite, ce qui peut contrevenir à la législation, notamment après plusieurs décisions judiciaires en Allemagne et en Autriche.

En Suisse, la Loi fédérale sur la protection des données (nLPD) entrée en vigueur en 2023 est inspirée du RGPD et adopte une définition large des données personnelles, incluant également les adresses IP.

Vous pouvez désactiver complètement le chargement externe en ajoutant ceci dans `mkdocs.yml` :

```yaml
theme:
  name: material
  font: false
```

Cela forcera l’utilisation des polices système locales. Vous pouvez télécharger les fichiers depuis Google Fonts et les héberger dans le dossier `docs/` ou un sous-dossier (ex. `docs/assets/`), puis les intégrer via CSS (voir section précédente).

!!! tip "Plugin intégré"
    Material for MkDocs propose un plugin intégré pour renforcer la confidentialité. Il désactive automatiquement les appels externes (Google Fonts, scripts, etc.) :
    ```yaml
      plugins:
        - privacy
    ```

Une autre possibilité est d’ajouter un formulaire de consentement aux cookies (bannière ou popup) afin de respecter la législation sur la protection des données :

```yaml
extra:
  consent:
    title: Accepter les cookies
    description: >- 
      Ce site utilise des cookies ou des services externes
      susceptibles de collecter des données personnelles.
```

---

## Personnaliser le logo et les icônes

Material for MkDocs permet de personnaliser l’identité visuelle de votre site en ajoutant un logo, une favicon (icône affichée dans l’onglet du navigateur), et même des icônes personnalisées dans le menu de navigation ou les liens sociaux.

Pour ajouter un logo et une favicon, placez vos fichiers image (de préférence au format .png ou .svg) dans le dossier `docs/` ou un sous-dossier (ex. `docs/assets/`), puis indiquez leur chemin relatif dans `mkdocs.yml` :

```yaml
theme:
  name: material
  logo: assets/images/logo.svg
  favicon: assets/images/favicon.png
```

---

## Personnalisation de la navigation

Material for MkDocs permet d’ajouter des fonctions avancées de navigation grâce à l’option `features:` dans le fichier `mkdocs.yml`.  
Voici trois options particulièrement utiles pour améliorer la structure et l’ergonomie du site.

### Organisation de la navigation

La clé `nav:` dans mkdocs.yml permet de définir l'ordre, la structure et les titres des pages dans la barre latérale (ou dans les onglets si `navigation.tabs` est activé).

Sans cette section, MkDocs génère automatiquement la navigation en fonction des fichiers présents dans le dossier `docs/`, mais cela donne souvent une structure désordonnée, surtout pour des projets complexes ou multilingues.

Ainsi, vous pourrez choisir l'ordre d'affichage des pages ou en omettre, les renommer sans modifier le titre réel du fichier `.md` et créer des sous-menus dans l'arborescence.

```yaml
nav:
  - Accueil: index.md
  - À propos:
      - Présentation: about/overview.md
      - Installation: about/installation.md
```

### Onglets de navigation

L'option `navigation.tabs` active l'affichage de la navigation principale sous forme d'onglets en haut de la page. Chaque élément de premier niveau dans `nav:` devient un onglet horizontal.

```yaml
theme:
  name: material

  features:
    - navigation.tabs
```

### Sections

L'option `navigation.sections` fonctionne en complément de `navigation.tabs`. Elle permet d’afficher les titres de sections dans le menu latéral gauche, en les groupant visuellement.

```yaml
theme:
  name: material

  features:
    - navigation.tabs
    - navigation.sections
```

### Pied de page

L’option `navigation.footer` permet d’afficher :

- Un pied de page de navigation avec des boutons *Page précédente* et *Page suivante*
- Un pied de page personnalisé, contenant des liens, icônes et du texte (notamment les crédits ou les mentions légales)

```yaml
theme:
  name: material

  features:
    - navigation.footer

extra:
  social:
    - icon: material/github
      link: https://github.com/mon-projet
    - icon: material/web
      link: https://exemple.com

copyright: Copyright &copy; 2025 Mon Organisation
```

---

## Exemple de configuration

```yaml title="mkdocs.yml"
theme:
  name: material

  font:
    text: Source Sans 3
    code: Source Code Pro

  logo: assets/images/logo.svg
  favicon: assets/images/favicon.png

  features:
    - navigation.footer
    - navigation.tabs
    - navigation.sections

  palette:
    # Mode sombre
    - scheme: slate
      toggle:
        icon: material/weather-sunny
        name: Passer en mode clair
      primary: indigo
      accent: red

    # Mode clair
    - scheme: default
      toggle:
        icon: material/weather-night
        name: Passer en mode sombre
      primary: blue
      accent: red

nav:
  - Accueil: index.md
  - À propos:
      - Présentation: about/overview.md
      - Installation: about/installation.md

plugins:
  - privacy

extra:
  social:
    - icon: material/github
      link: https://github.com/mon-projet
    - icon: material/web
      link: https://exemple.com

copyright: Copyright &copy; 2025 Mon Organisation

markdown_extensions:
  - attr_list
  - pymdownx.emoji:
      emoji_index: !!python/name:material.extensions.emoji.twemoji
      emoji_generator: !!python/name:material.extensions.emoji.to_svg
```

