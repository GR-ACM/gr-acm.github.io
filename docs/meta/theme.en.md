# Visual customization

This section presents different ways to customize the appearance and behavior of your MkDocs site, relying on the many features offered by **Material for MkDocs**.

---

## Changing the site's appearance

Material for MkDocs allows you to define custom color schemes and offer users a toggle button to switch between light and dark mode.

### Selecting the theme

Material for MkDocs comes with two themes: a light mode `default` and a dark mode `slate`.

In your `mkdocs.yml` file, add or modify the `theme:` section as follows:

```yaml
theme:
  name: material
  palette:
    scheme: slate
```

### Defining colors

The main colors of the theme are defined using `primary` and `accent`.

`primary` defines the dominant color of the theme. It's used in the top navigation bar, active links, and some interactive elements or buttons.

```yaml
theme:
  name: material
  palette:
    primary: deep purple
```

`accent` defines the secondary color. It's used to draw attention to elements like hovered links, selected icons, and visual effects (highlighting, selection indicators, etc.).

```yaml
theme:
  name: material
  palette:
    accent: pink
```

You can choose from a list of predefined colors provided by the theme such as `blue`, `cyan`, `green`, `teal`, `deep orange`, etc.
[See the full list here](https://squidfunk.github.io/mkdocs-material/setup/changing-the-colors/#primary-color).

You can also use custom colors by defining your own palette (with hex codes or CSS variables).
[See the official documentation](https://squidfunk.github.io/mkdocs-material/setup/changing-the-colors/#custom-colors).

### Enabling light/dark mode

Offering a light and dark color palette makes your documentation pleasant to read at different times of the day, so the user can choose accordingly.

The following configuration will render a toggle button next to the search bar to switch between themes.

!!! tip "Customize light/dark modes"
    You can also define different `primary` and `accent` colors for each palette (light or dark).
  
```yaml
theme:
  name: material

  palette:
    - scheme: slate
      toggle:
        icon: material/weather-sunny
        name: Switch to light mode

    - scheme: default
      toggle:
        icon: material/weather-night
        name: Switch to dark mode
```

The button uses Material Design icons, which can be customized using the `toggle:icon` property.

By default, it uses:

- `material/weather-night` :material-weather-night: to switch to dark mode;
- `material/weather-sunny` :material-weather-sunny: to return to light mode.

You can replace these icons with any from the Material Design Icons library.
[See the full icon list here](https://squidfunk.github.io/mkdocs-material/reference/icons-emojis/).

---

## Changing the font

Material for MkDocs makes it easy to change fonts via direct integration with [Google Fonts](https://fonts.google.com/). You can customize the text font (regular body) and the monospace font (used in code blocks), or even [load your own fonts](https://squidfunk.github.io/mkdocs-material/setup/changing-the-fonts/#additional-fonts) if needed.

```yaml
theme:
  name: material
  font:
    text: Roboto
    code: Roboto Mono
```

### Using icons and emojis

Material for MkDocs allows easy integration of icons and emojis directly in your Markdown content, via the `pymdownx.emoji` extension. Add the following configuration to the `markdown_extensions` section of your `mkdocs.yml` file:

```yaml
markdown_extensions:
  - attr_list
  - pymdownx.emoji:
      emoji_index: !!python/name:material.extensions.emoji.twemoji
      emoji_generator: !!python/name:material.extensions.emoji.to_svg
```

This allows you to use emojis in your content that are rendered as styled SVG icons, compatible with the theme and responsive to light/dark modes.
[See the full list of icons and emojis here](https://squidfunk.github.io/mkdocs-material/reference/icons-emojis/).

!!! warning ""My editor shows a syntax error!""
    This is expected. The syntax `!!python/name:...` is not standard YAML and is only understood by PyYAML (used by MkDocs). You can safely ignore the warning.

### Data protection

Using Google Fonts implies that the user's browser sends a request to a Google server to load the fonts. This can raise privacy concerns.

In Europe, the General Data Protection Regulation (GDPR) considers the user's IP address as personal data. Loading Google Fonts from a third-party server (such as Google’s) transmits this IP address without explicit consent, which may violate the law — notably after court decisions in Germany and Austria.

In Switzerland, the new Federal Act on Data Protection (nFADP) effective from 2023 is inspired by the GDPR and also treats IP addresses as personal data.

You can disable external loading entirely by adding this to `mkdocs.yml`:

```yaml
theme:
  name: material
  font: false
```

This forces the site to use local system fonts. You can also download the fonts from Google Fonts, place them in the `docs/` folder (or subfolder like `docs/assets/`), and include them via CSS (see previous section).

!!! tip "Built-in plugin"
    Material for MkDocs provides a built-in plugin to enhance privacy. It automatically disables external calls (Google Fonts, scripts, etc.):
    ```yaml
      plugins:
        - privacy
    ```
Another option is to add a cookie consent form (banner or popup) to comply with data protection laws:

```yaml
extra:
  consent:
    title: Accept cookies
    description: >-
      This site uses cookies or external services
      that may collect personal data.
```

---

## Customizing the logo and icons

Material for MkDocs lets you customize your site's visual identity by adding a logo, a favicon (browser tab icon), and even custom icons in the navigation or social links.

To add a logo and favicon, place your image files (preferably .png or .svg) in the `docs/` folder or a subfolder (e.g., `docs/assets/`), then reference them in `mkdocs.yml`:

```yaml
theme:
  name: material
  logo: assets/images/logo.svg
  favicon: assets/images/favicon.png
```

---

## Navigation customization

Material for MkDocs offers advanced navigation features through the `features:` option in the `mkdocs.yml` file.
Here are three particularly useful options to improve your site’s structure and usability.

### Navigation structure

The `nav:` key in mkdocs.yml lets you define the order, structure, and titles of pages in the sidebar (or tabs, if `navigation.tabs` is enabled).

Without this section, MkDocs generates the navigation based on files found in the `docs/` folder, which often leads to a messy structure — especially for complex or multilingual projects.

This lets you control page order, omit pages, rename navigation labels without changing file names, and create submenus.

```yaml
nav:
  - Home: index.md
  - About:
      - Overview: about/overview.md
      - Installation: about/installation.md
```

### Navigation tabs

The `navigation.tabs` option displays the main navigation as horizontal tabs at the top of the page. Each top-level item in `nav:` becomes a horizontal tab.

```yaml
theme:
  name: material

  features:
    - navigation.tabs
```

### Sections

The `navigation.sections` option complements `navigation.tabs`. It displays section headers in the left sidebar menu, visually grouping the content.

```yaml
theme:
  name: material

  features:
    - navigation.tabs
    - navigation.sections
```

### Footer

The `navigation.footer` option enables:

- A navigation footer with *Previous page* and *Next page* buttons
- A custom footer with links, icons, and text (credits, legal notices, etc.)

```yaml
theme:
  name: material

  features:
    - navigation.footer

extra:
  social:
    - icon: material/github
      link: https://github.com/my-project
    - icon: material/web
      link: https://example.com

copyright: Copyright &copy; 2025 My Organization
```

---

## Example configuration

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
    # Dark mode
    - scheme: slate
      toggle:
        icon: material/weather-sunny
        name: Switch to light mode
      primary: indigo
      accent: red

    # Light mode
    - scheme: default
      toggle:
        icon: material/weather-night
        name: Switch to dark mode
      primary: blue
      accent: red

nav:
  - Home: index.md
  - About:
      - Overview: about/overview.md
      - Installation: about/installation.md

plugins:
  - privacy

extra:
  social:
    - icon: material/github
      link: https://github.com/my-project
    - icon: material/web
      link: https://example.com

copyright: Copyright &copy; 2025 My Organization

markdown_extensions:
  - attr_list
  - pymdownx.emoji:
      emoji_index: !!python/name:material.extensions.emoji.twemoji
      emoji_generator: !!python/name:material.extensions.emoji.to_svg
```