# Publier le site

Après avoir créé et personnalisé votre site en local, vous pouvez le mettre en ligne gratuitement avec GitHub Pages. Cette section vous explique comment faire la première publication, puis comment le mettre à jour facilement par la suite.

---

## Prérequis

Assurez-vous d’avoir :

- [:simple-github: Un compte GitHub](http://github.com/)
- [:simple-github: Un dépôt public ou privé](http://github.com/) (ex. : `nom-utilisateur/mon-wiki`)
- [:simple-materialformkdocs: Un projet MkDocs fonctionnel avec un fichier `mkdocs.yml`](../installation/#initialiser-un-nouveau-site-mkdocs)
- [:simple-python: Activé l’environnement virtuel Python (venv)](../installation/#activer-lenvironnement-virtuel), si ce n’est pas déjà fait.

---

## Fichier de configuration pour Github Actions

Dans un dossier `.github/workflows/`, créez un fichier nommé `ci.yml` :

```yaml
name: ci 
on:
  push:
    branches:
      - master 
      - main
permissions:
  contents: write
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Configure Git Credentials
        run: |
          git config user.name github-actions[bot]
          git config user.email 41898282+github-actions[bot]@users.noreply.github.com
      - uses: actions/setup-python@v5
        with:
          python-version: 3.x
      - run: echo "cache_id=$(date --utc '+%V')" >> $GITHUB_ENV 
      - uses: actions/cache@v4
        with:
          key: mkdocs-material-${{ env.cache_id }}
          path: ~/.cache 
          restore-keys: |
            mkdocs-material-
      - run: pip install mkdocs-material 
      - run: mkdocs gh-deploy --force
```

**Si vous utilisez le plugin `mkdocs-static-i18n` :**

Dans la section `steps:` avant l’appel à `mkdocs gh-deploy` :

```yaml title="ci.yml" hl_lines="3"
steps:
  - run: pip install mkdocs-material
  - run: pip install mkdocs-static-i18n
  - run: mkdocs gh-deploy --force
```

---

## Déployer le site sur Github Pages

Si nécessaire, arrêtez le serveur en appuyant dans le terminal :

=== ":material-microsoft-windows: Windows"
    Appuyer simultanément sur ++ctrl+c++

=== ":material-apple: macOS"
    Appuyer simultanément sur ++cmd+c++

Initialisez un dépôt Git dans le dossier du projet à l'aide de cette commande, dans le terminal :

```bash
git init
```

Optionnellement, vous pouvez ajouter un fichier `.gitignore` à la racine de votre projet.  
Le fichier `.gitignore` est un fichier spécial utilisé par Git pour lui dire quels fichiers ou dossiers il ne doit pas pousser sur GitHub.

Ajoutez tous les fichiers modifiés ou nouveaux du dossier courant et de ses sous-dossiers à la zone de préparation avec la commande :

```bash
git add .
```

Optionnellement, vous pouvez contrôler ce que vous venez d'ajouter au prochain commit avec la commande :

```bash
git status
```

Vous pouvez maintenant lancer le commit avec la commande (`-m "..."` détermine le message d'accompagnement) :

```bash
git commit -m "Initial commit"
```

Entrez la commande suivante, avec l'adresse de votre dépôt Github. Vous pouvez la copier directement depuis la page de votre dépôt.

```bash
git remote add origin git#@github.com:nom-utilisateur/mon-wiki.git
```

Vous pouvez maintenant pousser vos fichiers vers le dépôt :

```bash
git push origin main
```

À partir de la page de votre dépôt :  

- Cliquez sur *Settings* > *Pages*
- Sous *Build and deployment* :

    - Source: `Deploy from a branch`
    - Branch: `gh-pages`

- Cliquez sur `Save`.

Dans `Actions`, un nouveau workflow est apparu pour le déploiement de votre site. Lorsque votre site sera déployé, une adresse `https://nom-utilisateur.github.io/mon-wiki` apparaîtra. Cette action peut prendre quelques minutes.

Félicitations, vous avez déployé votre site!

---

## Mettre à jour le site

Une fois vos modifications terminées, déployez à nouveau votre site avec la commande :

!!! tip "Optionnel"
  Si vous avez créé ou modifié votre fichier `.gitignore`, vous devrez supprimer les fichiers de l'index Git. Le fichier `.gitignore` empêche Git de suivre de nouveaux fichiers correspondants, mais n'a aucun effet sur les fichiers déjà suivis.
  ```bash
  git rm -r --cached .
  ```

```bash
git add .
```

```bash
git commit -m "Mise à jour"
```

```bash
git push origin main
```

La nouvelle version est automatiquement publiée sur la branche `main` et sur la branche `gh-pages` de votre dépôt.