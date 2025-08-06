# Publishing the Site

After creating and customizing your site locally, you can publish it for free with GitHub Pages. This section explains how to make the first deployment, then how to update the site easily afterward.

---

## Prerequisites

Make sure you have:

- [:simple-github: A GitHub account](http://github.com/)
- [:simple-github: A public or private repository](http://github.com/) (e.g., `username/my-wiki`)
- [:simple-materialformkdocs: A working MkDocs project with a `mkdocs.yml` file](../installation/#initialize-a-new-mkdocs-site)
- [:simple-python: Activated the Python virtual environment (venv)](../installation/#activate-the-virtual-environment), if not already done.

---

## GitHub Actions Configuration File

In a `.github/workflows/` folder, create a file named `ci.yml`:

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

**If you are using the `mkdocs-static-i18n` plugin:**

In the `steps:` section, before the call to `mkdocs gh-deploy`:

```yaml title="ci.yml" hl_lines="3"
steps:
  - run: pip install mkdocs-material
  - run: pip install mkdocs-static-i18n
  - run: mkdocs gh-deploy --force
```

## Deploying the Site on GitHub Pages

If necessary, stop the local server by pressing in the terminal:

=== ":material-microsoft-windows: Windows"
    Press ++ctrl+c++

=== ":material-apple: macOS"
    Press ++cmd+c++

Initialize a Git repository in the project folder using the terminal:

```bash
git init
```

Optionally, you can add a `.gitignore` file at the root of your project.
The `.gitignore` file is used by Git to specify which files or folders should not be pushed to GitHub.

Add all new or modified files in the current folder and its subfolders with:

```bash
git add .
```

Optionally, you can verify what has been added with:

```bash
git status
```

Now commit your changes with the following command (`-m "..."` specifies the commit message):

```bash
git commit -m "Initial commit"
```

Next, enter the following command with the address of your GitHub repository. You can copy it directly from your repository page.

```bash
git remote add origin git@github.com:username/my-wiki.git
```

Push your files to the repository:

```bash
git push origin main
```

From your repository page:

- Click *Settings* > *Pages*
- Under *Build and deployment*:

    - Source: `Deploy from a branch`
    - Branch: `gh-pages`

- Click `Save`.

A new workflow will appear in the `Actions` tab for deploying your site. Once deployed, a URL like `https://username.github.io/my-wiki` will appear. This process may take a few minutes.

Congratulations, your site is now live!

---

## Updating the Site

After making your changes, redeploy your site with:

```bash
mkdocs gh-deploy
```

The new version will be published automatically to the gh-pages branch of your repository, updating your site.

To delete the local `site/` folder before rebuilding the site, use:

```bash
mkdocs gh-deploy --clean
```

This can be useful if:

- You have deleted or renamed pages
- You want to ensure no obsolete files are kept in `site/`

!!! info
    With `mkdocs gh-deploy`, this step is rarely necessary because MkDocs uses a temporary folder for deployment.
    Use `--clean` only if you're unsure or after major structural changes.