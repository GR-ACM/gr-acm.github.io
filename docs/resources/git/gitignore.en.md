# Excluding Files in a MkDocs Repository

[:material-account: Muong, Kethsana](https://infoscience.epfl.ch/entities/person/894bf3c8-287b-4ae5-a1fb-63dd69db6e5d)  
:material-calendar: August 2025  
:material-office-building: Archives de la construction moderne – EPFL

*This file is distributed under the GNU General Public License v3. You may redistribute and/or modify it under the terms of the GNU GPL as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.*  
*This `.gitignore` file is provided as an example for projects using [MkDocs](https://www.mkdocs.org/) with the [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) theme. It includes common directories, temporary files, and build artifacts that should be excluded from Git tracking during site development and deployment.*  
*To obtain a copy of the GNU General Public License, visit [www.gnu.org/licenses/](https://www.gnu.org/licenses/).*

---

## Purpose

The `.gitignore` file tells Git which files or folders should **not** be tracked or included in the repository history.  
It is essential to prevent accidentally committing:

- large or automatically generated files (e.g., `/site/`)
- temporary or cache files (e.g., `__pycache__/`, `.DS_Store`)
- files containing sensitive information (e.g., `.env`)

!!! tip "Reminder"
    The `.gitignore` file prevents Git from tracking **new** matching files, but has no effect on files that are already tracked.  
    If you add a rule after files are already committed, remove them from the index using:
    ```bash
    git rm -r --cached .
    git add .
    git commit -m "Clean ignored files from repository"
    ```

## Sample `.gitignore` File

```py
# =========================
# MkDocs and deployment
# =========================

# MkDocs-generated Folder
site/

# Plugins Cache
.cache/

# =========================
# Python environment
# =========================

# Environnements virtuels
.env
.venv
env/
venv/
ENV/
env.bak/
venv.bak/

# Python Cache
__pycache__/
*.py[cod]
*.pyo
*.pyd
*.pyc

# Pip logs
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
# IDEs / Code editors
# =========================

# Visual Studio Code / VSCodium
.vscode/
.history/

# Save or temporary IDE files
*~
*.swp
*.bak
*.tmp

# =========================
# System files
# =========================

# macOS
.DS_Store

# Fichiers log génériques
*.log
```