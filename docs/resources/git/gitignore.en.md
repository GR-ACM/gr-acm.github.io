# `.gitignore` file

The `.gitignore` file tells Git which files or folders should **not** be tracked or included in your repository history.  
It’s essential to prevent accidentally committing:

- large or auto-generated files (e.g., `/site/`)
- temporary or cache files (e.g., `__pycache__/`, `.DS_Store`)
- sensitive or environment-specific files (e.g., `.env`)

!!! tip "Reminder"
    The `.gitignore` file prevents Git from tracking new matching files, but it doesn't remove those already tracked. If a file is already being tracked, updating the `.gitignore` won’t remove it.
    You’ll need to untrack it manually with:
    ```bash
    git rm -r --cached .
    git add .
    git commit -m "Clean ignored files"
    ```

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