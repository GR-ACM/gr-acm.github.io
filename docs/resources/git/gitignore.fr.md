# Exclusion de fichiers dans un dépôt MkDocs

[:material-account: Muong, Kethsana](https://infoscience.epfl.ch/entities/person/894bf3c8-287b-4ae5-a1fb-63dd69db6e5d)  
:material-calendar: Août 2025  
:material-office-building: Archives de la construction moderne – EPFL

*Ce fichier est distribué sous la licence GNU General Public License v3. Vous pouvez le redistribuer et/ou le modifier selon les termes de la GNU GPL tels que publiés par la Free Software Foundation, soit la version 3 de la licence, soit (à votre choix) toute version ultérieure.*    
*Ce fichier `.gitignore` est proposé à titre d’exemple pour des projets utilisant [MkDocs](https://www.mkdocs.org/) avec le thème [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/). Il contient une liste de répertoires, fichiers temporaires et artefacts courants à exclure du suivi Git lors du développement ou du déploiement d’un site statique.*  
*Pour obtenir une copie de la GNU General Public License, consultez [www.gnu.org/licenses/](https://www.gnu.org/licenses/).*

---

## Objectif

Le fichier `.gitignore` permet d’indiquer à Git quels fichiers ou dossiers ne doivent **pas** être suivis ni enregistrés dans l’historique du dépôt.  
C’est un outil essentiel pour éviter d’ajouter accidentellement :

- des fichiers volumineux ou générés automatiquement (ex: `/site/`)
- des fichiers temporaires ou de cache (ex: `__pycache__/`, `.DS_Store`)
- des fichiers contenant des informations sensibles (ex: `.env`)

!!! tip "Rappel"
    Le fichier `.gitignore` empêche Git de suivre de nouveaux fichiers correspondants, mais n'a aucun effet sur les fichiers déjà suivis. Si vous ajoutez une règle après coup, vous devrez supprimer ces fichiers de l’index avec :
    ```bash
    git rm -r --cached .
    git add .
    git commit -m "Nettoyage des fichiers ignorés"
    ```

## Fichier `.gitignore`

```py
# =========================
# MkDocs et déploiement
# =========================

# Dossier généré automatiquement par MkDocs (site statique)
site/

# Cache généré par des plugins comme `privacy`
.cache/

# =========================
# Environnement Python
# =========================

# Environnements virtuels
.env
.venv
env/
venv/
ENV/
env.bak/
venv.bak/

# Cache Python
__pycache__/
*.py[cod]
*.pyo
*.pyd
*.pyc

# Fichiers de log ou d’erreur pip
pip-log.txt
pip-delete-this-directory.txt

# =========================
# Packaging & distribution
# =========================

.Python
build/
develop-eggs/
dist/
downloads/
eggs/
.eggs/
lib/
lib64/
parts/
sdist/
var/
wheels/
pip-wheel-metadata/
share/python-wheels/
*.egg-info/
.installed.cfg
*.egg
MANIFEST

# =========================
# IDEs / Éditeurs
# =========================

# Visual Studio Code / VSCodium
.vscode/
.history/

# Fichiers de sauvegarde ou temporaires d’éditeurs
*~
*.swp
*.bak
*.tmp

# =========================
# Système / fichiers inutiles
# =========================

# macOS
.DS_Store

# Fichiers log génériques
*.log
```