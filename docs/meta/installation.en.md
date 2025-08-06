# Installation

This wiki was created using Material for MkDocs. It is hosted on GitHub Pages.

This documentation describes how this wiki was deployed. Its goal is to document the technical steps taken during its creation while providing a reproducible guide for anyone wishing to build a similar site.

This guide is based directly on the [official Material for MkDocs documentation](https://squidfunk.github.io/mkdocs-material/getting-started/), and serves as a simplified, condensed version adapted to this project's context.

---

## Prerequisites

Before beginning the installation, make sure you have the following tools:

- [:simple-python: Python](https://www.python.org/downloads/), a simple, readable, and widely used programming language. We recommend version 3.4 or newer for this guide.

    - [:material-language-python: pip](https://pip.pypa.io/en/stable/installation/), Python’s package manager. It is used to install libraries and tools, and is included by default with Python 3.4+.

- :fontawesome-solid-file-code: A code editor. We recommend [:simple-vscodium: VSCodium](https://vscodium.com/), an open-source alternative to [:material-microsoft-visual-studio-code: Visual Studio Code](https://code.visualstudio.com/).

- [:simple-github: A GitHub account](http://github.com/), since we’ll deploy the site using the [GitHub Pages](https://pages.github.com/) service.

!!! tip "Tip"
    Once installed, you can check if Python is working by opening a terminal and typing:

        python --version

    To check if `pip` is working:

        pip --version

---

## Initial installation

Open a terminal in the folder where you want to create your project:

=== ":material-microsoft-windows: Windows"
    Open :material-powershell: Powershell

=== ":material-apple: macOS"
    Open :material-console: Terminal

The terminal runs commands inside a specific folder. It’s important to navigate to the folder where you want to create your wiki.  
The `cd` ("change directory") command lets you move through your file structure from the terminal.

```bash
cd path/to/my/folder
```

!!! tip "Run commands from the editor"
    Visual Studio Code and VSCodium include an integrated terminal, allowing you to continue running commands directly from the editor. Go to the **Terminal** menu > **New Terminal**, or use the shortcut.

### Create a Python virtual environment

!!! note inline end "What does this command mean?"
    `-m venv` uses Python’s built-in venv module to create virtual environments.
    `venv` is the name of the folder that will be created in your project (you can change it, but `venv` is a common convention).

In the terminal, type the following command:

```bash
python -m venv venv
```

### Activate the virtual environment

=== ":material-microsoft-windows: Windows"
    Type `.\venv\Scripts\activate`

=== ":material-apple: macOS"
    Type `source venv/bin/activate`

!!! success "Activation"
    If successful, you’ll see the environment name `(venv)` appear at the start of your command line.

Each time you close the terminal, you’ll need to reactivate the environment with the same command as above.

### Install Material for MkDocs with `pip`

```bash
pip install mkdocs-material
```

This command installs MkDocs, Material for MkDocs, and all required dependencies.

After installation, you can verify that MkDocs is available by typing `mkdocs --version`.

### Open the project folder in Visual Studio Code or VSCodium

!!! note inline end "What does this command mean?"
    `code` launches Visual Studio Code / VSCodium.
    `.` means “the current folder”.

You can use the following command in your terminal (from the correct folder):

```bash
code .
```

### Initialize a new MkDocs site

In the terminal (with the virtual environment still active), type:

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

- `docs/index.md`: the site’s homepage. Written in Markdown, you can edit it freely.
- `mkdocs.yml`: the configuration file, written in YAML (for title, theme, navigation, languages, etc.).

To add a new page to your site, simply create a `.md` file in the `docs/` folder.
For example, if you create a file called `about.md`, it will be available at https://my-site/about/.

!!! note "Need a Markdown syntax refresher?"
    Check out our [Markdown syntax guide](../structure/#markdown) for the basics.

### Configure the site in `mkdocs.yml`

Open `mkdocs.yml` in your editor and replace its content with the following configuration:


```yaml title="mkdocs.yml"
site_name: My project
site_url: https://my-site.example
theme:
  name: material
```

---

## Preview the site locally

Now that your site is configured, you can build and preview it live while you work.

In the terminal (with the virtual environment activated), type:

```bash
mkdocs serve
```

Open a browser and go to: [http://localhost:8000/](http://localhost:8000/)

!!! tip "Live preview your changes"
    As long as `mkdocs serve` is running, the site remains visible and updates automatically when you save changes.

When you're done, stop the server by pressing the following in the terminal:

=== ":material-microsoft-windows: Windows"
    Press ++ctrl+c++

=== ":material-apple: macOS"
    Press ++cmd+c++

---

## YAML schema validation

!!! tip ""
    The `mkdocs.yml` file is central to your site's configuration. To avoid syntax or structure errors (which may prevent the site from building), we strongly recommend enabling automatic YAML schema validation in your code editor.

In Visual Studio Code or VSCodium, search for YAML in the Extensions panel and install the official YAML extension by Red Hat.

Look for the `settings.json` file in the settings panel or create one at the root of your project.

Add the following code to the file:

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

---

## Deployment

Once your site is created and customized locally, you can publish it for free using GitHub Pages.
See the [Publishing section](../publish) to learn how to deploy your site for the first time, and how to update it easily afterward.