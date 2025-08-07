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

!!! tip "`.gitignore` file"
    Optionally, you can add a `.gitignore` file at the root of your project.
    The `.gitignore` file is used by Git to specify which files or folders should not be pushed to GitHub.  
    To learn more, see the [dedicated page](/en/resources/gitignore/).
    You can also download this site's `.gitignore` file directly from the GitHub repository.

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

Once your changes are complete, redeploy your site using the following commands:

??? tip "If you added a `.gitignore` file"
    If you've added or updated your .gitignore file, you may need to remove existing files from Git's index.
    The .gitignore file prevents Git from tracking new matching files, but it doesn't remove those already tracked.
    ```bash
    git rm -r --cached .
    ```

Then stage and commit your changes:

```bash
git add .
```

```bash
git commit -m "Mise à jour"
```

```bash
git push origin main
```

Your updated site will automatically be published to both the `main` and `gh-pages` branches of your repository.  
No additional steps are needed. As long as your repository is configured to deploy from the `gh-pages branch`, your site will be updated automatically.

---

## Troubleshooting

Here are some common issues you might encounter when updating or deploying your site with Git and MkDocs, along with solutions to fix them.

### Error : `failed to push some refs`

While updating, you might see the following error:

```bash
git push origin main
To https://github.com/utilisateur/mon-repo.git
! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'https://github.com/utilisateur/mon-repo.git'
hint: Updates were rejected because the remote contains work that you do not have locally.
hint: This is usually caused by another repository pushing to the same ref.
hint: If you want to integrate the remote changes, use
hint: 'git pull' before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

**Cause**: You're trying to push changes (`git push`) while the remote `main` branch already contains commits that are not in your local version — for example, if you edited a file directly on GitHub (like `README.md`).

**Solution**: Run a `git pull` to retrieve the latest changes from the remote branch before pushing again:

```bash
git pull origin main --rebase
git push origin main
```

!!! note "What does `--rebase` do?"
    The `--rebase` option integrates the remote changes into your local history in a clean way, avoiding unnecessary merge commits.