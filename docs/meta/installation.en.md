# Installation

This documentation describes how this wiki was deployed. Its purpose is to document the technical steps followed during its creation, while also providing a reproducible guide for those who wish to build a similar site.

This guide is based directly on the [official Material for MkDocs documentation](https://squidfunk.github.io/mkdocs-material/getting-started/), and is a condensed, simplified version tailored to the context of this project.

---

## Prerequisites

Before starting the installation, make sure you have the following tools available:

- [:simple-python: Python](https://www.python.org/downloads/), a simple, readable, and widely used programming language. We recommend version 3.4 or later for this guide.

    - [:material-language-python: pip](https://pip.pypa.io/en/stable/installation/), Python's package manager. It allows you to install libraries and tools. It is included by default with Python 3.4 and later.

- :fontawesome-solid-file-code: A code editor. We recommend [:simple-vscodium: VSCodium](https://vscodium.com/), an open-source alternative to [:material-microsoft-visual-studio-code: Visual Studio Code](https://code.visualstudio.com/).

- [:simple-github: A GitHub account](http://github.com/), since we’ll be deploying the site via [GitHub Pages](https://pages.github.com/).

!!! tip
    Once installed, you can check if Python works by opening a terminal and typing:

        python --version
        
    To check if `pip` works:

        pip --version

---

## Initial Setup

Open a terminal in the folder where you want to create the project:

- On :material-microsoft-windows: Windows: :material-powershell: Powershell
- On :material-apple: macOS: :material-console: Terminal

The terminal runs commands within a specific folder. It's important to navigate to the folder where you want to create your wiki.  
The `cd` command (“change directory”) lets you move through the folder structure from the terminal.

```bash
cd path/to/my/folder
```

!!! tip "Run commands directly from the editor"
    Visual Studio Code and VSCodium include an integrated terminal, which allows you to continue typing commands directly from within the editor.  
    Go to the **Terminal** menu > **New Terminal**, or use the keyboard shortcut.

### Create a Python virtual environment

!!! note inline end "What does this command mean?"
    `-m venv` uses the built-in venv module, which is used to create virtual environments.
    `venv` is the name of the folder that will be created in your project (you can choose another name, but `venv` is a common convention).

In the terminal, type the following command:

```bash
python -m venv venv
```
### Activate the virtual environment

- On :material-microsoft-windows: Windows: .\venv\Scripts\activate
- On :material-apple: macOS: source venv/bin/activate

!!! success "Activation"
    If everything works correctly, you’ll see the environment name `(venv)` appear at the beginning of your command prompt.

If you close the terminal, you’ll need to reactivate the environment next time using the same command as above.

### Install Material for MkDocs using `pip`

```bash
pip install mkdocs-material
```

This command installs MkDocs, Material for MkDocs, and all the necessary dependencies.

After installation, you can check that MkDocs is available by typing `mkdocs --version`.

### Open the project folder in Visual Studio Code or VSCodium

!!! note inline end "What does this command mean?"
    `code` calls the Visual Studio Code / VSCodium application,
    `.` means "current folder".

You can use the following command in your terminal (while inside the correct folder):

```bash
code .
```

### Initialize a new MkDocs site

In the terminal (with the virtual environment still activated), type the following command:

```bash
mkdocs new .
```

The following structure should be created:

```pgsql
.
├─ docs/
│  └─ index.md
└─ mkdocs.yml
```

This command creates two important elements:

- `docs/index.md`: the home page of the site, written in Markdown. You can edit it freely.
- `mkdocs.yml`: a configuration file written in YAML (site title, theme, navigation, languages, etc.).

To add a new page to your site, simply create a new `.md` file in the `docs/` folder.
For example, creating a file named `about.md` in `docs/` will make it available at https://mysite/about/.

!!! note "Need a refresher on how to write in Markdown?"
    See our Markdown syntax guide for the basics.

### Configure the site with `mkdocs.yml`

Open `mkdocs.yml` in your editor and replace its content with the following configuration:

```yml title="mkdocs.yml"
site_name: My project documentation
site_url: https://mysite.exemple
theme:
  name: material
```

## Preview the site locally

Now that your site is configured, you can generate and preview it locally in real time as you work on it.

In the terminal (with the virtual environment activated), type:

```bash
mkdocs serve
```

Open a browser and go to: [http://localhost:8000/](http://localhost:8000/)

!!! tip "Preview your changes in real time"
    As long as the `mkdocs serve` command is running in your terminal, the site stays visible and updates automatically with each save.

When you're done, you can stop the server in the terminal by pressing:

- On :material-microsoft-windows: Windows : `Ctrl + C`
- On :material-apple: macOS : `Cmd + C`

## YAML Schema Validation

!!! tip ""
    The `mkdocs.yml` file is central to configuring your site. To avoid syntax or structural errors (which could block site generation), it’s strongly recommended to enable automatic YAML schema validation in your code editor.

For Visual Studio Code and VSCodium, search for *YAML* in the Extensions panel and install the official YAML extension by Red Hat.

Look for the `settings.json` file in your editor’s settings, or create one at the root of the project.

Add the following code to that file:

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