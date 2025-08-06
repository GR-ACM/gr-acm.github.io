# Structuration du contenu

---

## Markdown

Ce site est construit avec Material for MkDocs, qui utilise le langage Markdown pour écrire le contenu des pages. Markdown est un langage léger de balisage qui permet de formater du texte de manière simple, lisible et rapide, sans avoir besoin de connaître le HTML.  
Cette section s'appuie directement sur [:material-language-markdown-outline: *The Markdown Guide*](https://www.markdownguide.org/basic-syntax/).

Voici les principales règles de syntaxe Markdown à connaître :

### Titres

Markdown permet de structurer le contenu avec des niveaux de titres allant de `#` (niveau 1) à `######` (niveau 6). Material for MkDocs génère automatiquement la table des matières à partir des titres de niveau 2 (`##`) et 3 (`###`).

```markdown
# Titre de niveau 1
## Titre de niveau 2
### Titre de niveau 3
```

N'utilisez qu'un seul titre de niveau 1 (`#`) par page. Il est généralement utilisé pour le titre principal et repris automatiquement par MkDocs comme nom de page.

Utilisez une hiérarchie logique des titres (`##`, `###`, etc.) pour que la table des matières soit claire.

!!! tip "Compatibilité"
    Les applications Markdown ne gèrent pas toutes de la même façon l’absence d’espace entre les dièses (#) et le titre.
    Pour une compatibilité maximale :

    - Ajoutez un espace entre les `#` et le texte du titre.

        `## Mon titre` :octicons-check-circle-fill-24:

        `##Mon titre` :octicons-x-circle-fill-24:

    - Ajoutez une ligne vide avant et après chaque titre. Cela garantit un rendu stable dans tous les moteurs de rendu Markdown.

    ```markdown
    Ajoutez une ligne vide avant...
 
    ## Titre
    
    ...et après chaque titre ou bloc.
    ```

### Paragraphes

En Markdown, un paragraphe est simplement une ou plusieurs lignes de texte, séparées par une ligne vide. Par défaut, si vous appuyez simplement sur ++enter++ sans ajouter de ligne vide, le texte sera fusionné en un seul paragraphe.

Par exemple :

```markdown
Ligne 1
Ligne 2
```

Cela affichera une seule ligne continue, comme :  
Ligne 1 Ligne 2

Cependant, cela reste utile pour rendre votre fichier plus lisible.

Pour insérer un saut de ligne manuel sans créer un nouveau paragraphe, ajoutez deux espaces à la fin de la ligne :

```markdown
Ligne 1␣␣
Ligne 2
```

### Texte en évidence

Markdown permet de mettre en valeur du texte avec **le gras**, *l'italique* et ***les deux à la fois***.

```markdown
*italique*
**gras**
***italique et gras***
```

Vous pouvez aussi appliquer l’emphase à l’intérieur d’un mot, sans espaces, comme ceci :
`incroy**able**` → incroy**able**

### Listes

Markdown permet de créer des listes non ordonnées (puces) ou ordonnées (numérotées), avec une syntaxe simple. Pour imbriquer une sous-liste, ajoutez au moins 2 espaces ou une tabulation avant l’élément.

**Liste non ordonnée (puces)**

Pour créer une liste à puces, vous pouvez utiliser les symboles `-`, `*` et `+` devant chaque élément. Les trois fonctionnent, mais il est recommandé de rester cohérent dans un même fichier.

```markdown
- Élément 1
- Élément 2
    - Élément imbriqué
```

**Liste ordonnée (numérotée)**

Utilisez des chiffres suivis d’un point (`1.`, `2.`, etc.) pour créer une liste numérotée. Les numéros n’ont pas besoin d’être dans l’ordre (Markdown les renumérote automatiquement), mais la première ligne doit commencer par `1.`.  
Pour une meilleure compatibilité entre les moteurs Markdown, utilisez toujours le point `.` (et non une parenthèse) après les chiffres.

```markdown
1. Premier
2. Deuxième
    1. Élément imbriqué
3. Troisième
```

Si vous devez commencer une liste non ordonnée par un nombre suivi d’un point (par ex. "3.5 litres"), ajoutez un antislash (`\`) pour éviter que Markdown interprète cela comme une liste.

Si vous souhaitez un contrôle plus strict du comportement des listes (notamment l’imbrication), vous pouvez activer l’extension `sane_lists` dans `mkdocs.yml` :

```yaml title="mkdocs.yml"
markdown_extensions:
  - sane_lists
```

Markdown ne renumérotera plus automatiquement les éléments de liste.

### Liens et images

**Liens**

En Markdown, vous pouvez insérer facilement des liens cliquables avec une syntaxe simple.
Vous pouvez ajouter un titre optionnel, qui s'affichera au survol du lien. Pour cela, ajoutez-le entre guillemets, juste après l’URL.

```markdown
[Texte du lien](https://exemple.com "Ceci est un exemple")
```

Pas d’espace entre `[` et `(` dans les liens. Survoler le lien affichera : *Ceci est un exemple*.

!!! tip "Liens au format référence"
    Markdown permet aussi de définir des liens de manière plus lisible, en séparant le texte du lien de l’URL.
    Cela peut être utile dans des documents longs, pour ne pas surcharger les paragraphes avec des URLs.

    ```markdown
    [Documentation][material-mkdocs]

    [material-mkdocs]: https://squidfunk.github.io/mkdocs-material/
    ```

    Vous pouvez placer la référence (`[material-mkdocs]: ...`) n’importe où dans le fichier, généralement à la fin de la page.


Si vous souhaitez transformer rapidement une URL brute ou une adresse email en lien, vous pouvez simplement l’entourer de chevrons < > :

<adresse@exemple.com>

**Images**

Les images fonctionnent de manière similaire :

```markdown
![Texte alternatif](chemin/vers/image.png)
```

### Citations et code

En Markdown, vous pouvez insérer une citation ou un bloc de texte mis en retrait en utilisant le symbole `>` en début de ligne.
Vous pouvez également imbriquer plusieurs niveaux de citation.

```markdown
> Ceci est une citation.
>> Citation imbriquée.
```

> Ceci est une citation.
>> Citation imbriquée.

Material for MkDocs propose une alternative aux citations Markdown : les encadrés ou blocs informatifs.
Elles permettent de structurer visuellement l'information grâce à des icônes, des couleurs, et des types personnalisables (astuce, avertissement, note, etc.).

!!! quote "Exemple"
    Ceci est une citation.

Ce type d’encadré sera expliqué dans une [section dédiée](#encadres).

Pour mettre en forme un mot ou une phrase comme du code, entourez-le de backticks `` ` ``, aussi appelés accents graves.

```markdown
`code en ligne`
```

Si le mot ou la phrase contient déjà un backtick, vous pouvez l’échapper en utilisant deux backticks au lieu d’un seul.

!!! info "Et les blocs de code ?"
    Pour afficher plusieurs lignes de code dans un bloc dédié, utilisez des blocs de code multilignes.
    Ceux-ci sont pris en charge nativement par Material for MkDocs avec des fonctionnalités avancées (numérotation, copier-coller, surbrillance syntaxique, etc.)
    Ce sujet est traité dans [la section dédiée](#blocs-de-code).

---

## Material for MkDocs

En plus du Markdown standard, Material for MkDocs prend en charge de nombreuses extensions Markdown enrichies via `pymdown-extensions`, une suite puissante d’outils développée pour améliorer la lisibilité, la structuration et l’interactivité de votre documentation.

Ces fonctionnalités sont nativement intégrées ou facilement activables via le fichier `mkdocs.yml`, sous `markdown_extensions:`.

Cette section vous présente les principales extensions utiles pour rendre votre documentation plus claire, plus lisible et plus professionnelle.

### Blocs de code

Material for MkDocs prend en charge des blocs de code interactifs et stylisés avec de nombreuses options.

- Mise en surbrillance automatique (selon le langage)
- Numérotation des lignes
- Bouton “Copier” dans le coin du bloc

Pour activer ces options, modifiez/ajoutez ces éléments dans votre fichier `mkdocs.yml` :

```yaml title="mkdocs.yml"
features:
    - content.code.copy

markdown_extensions:
  - pymdownx.highlight:
      anchor_linenums: true
      line_spans: __span
      pygments_lang_class: true
  - pymdownx.inlinehilite
  - pymdownx.snippets
  - pymdownx.superfences
```

La syntaxe de base est identique au Markdown standard : utilisez trois accents graves (`` ``` ``), pour ouvrir et fermer le bloc, et ajoutez éventuellement le nom du langage pour activer la coloration syntaxique.

````markdown
```py
num = int(input("Enter a number: "))
if (num % 2) == 0:
   print("{0} is Even".format(num))
else:
   print("{0} is Odd".format(num))
```
````

```py
num = int(input("Enter a number: "))
if (num % 2) == 0:
   print("{0} is Even".format(num))
else:
   print("{0} is Odd".format(num))
```

Affichez un bandeau au-dessus du bloc avec le titre `title="..."`.

````markdown
```py title="odd_or_even.py"
num = int(input("Enter a number: "))
if (num % 2) == 0:
   print("{0} is Even".format(num))
else:
   print("{0} is Odd".format(num))
```
````

```py title="odd_or_even.py"
num = int(input("Enter a number: "))
if (num % 2) == 0:
   print("{0} is Even".format(num))
else:
   print("{0} is Odd".format(num))
```

De la même façon, ajoutez `linenums="..."` à la suite pour activer la numérotation des lignes et `hl_lines="..."` pour mettre en évidence une ou plusieurs lignes :

````markdown
```py title="odd_or_even.py" linenums="1" hl_lines="2-3"
num = int(input("Enter a number: "))
if (num % 2) == 0:
   print("{0} is Even".format(num))
else:
   print("{0} is Odd".format(num))
```
````

```py title="odd_or_even.py" linenums="1" hl_lines="2-3"
num = int(input("Enter a number: "))
if (num % 2) == 0:
   print("{0} is Even".format(num))
else:
   print("{0} is Odd".format(num))
```

Vous pouvez aussi spécifier plusieurs lignes ou plages :

````markdown
```py hl_lines="2 4 6-8"
````

### Encadrés

Les encadrés peuvent mettre en valeur une remarque, un avertissement, un exemple ou une information importante.

Pour les activer, modifiez/ajoutez ces éléments dans votre fichier `mkdocs.yml` :

```yaml title="mkdocs.yml"
markdown_extensions:
  - admonition
  - pymdownx.details
  - pymdownx.superfences
```

Les encadrés commencent par `!!!`, suivi d'un mot clé qui détermine le type.
Il est également possible de les rendre dépliables/repliables, avec `???` (replié) ou `???+` (déplié).
Par défaut, le titre est determiné par le type, mais vous pouvez le définir avec des guillemets.

```markdown
!!! info
    Ce wiki utilise Material for MkDocs.

??? warning "Attention"
    Attention à la casse dans vos noms de fichiers.
```

!!! info
    Ce wiki utilise Material for MkDocs.

??? warning "Attention"
    Attention à la casse dans vos noms de fichiers.

Il est également possible d'ajouter un encadré sur le côté avec `inline` ou `inline end `.

!!! example inline end "Exemple"
    Ce type d'encadré prend moins de place verticalement.

```markdown
!!! example inline end "Exemple"
    Cet encadré prend moins de place verticalement.
```

Types disponibles : `note`, `tip`, `quote`, `warning`, `danger`, `bug`, etc.
[Voir tous les types et styles](https://squidfunk.github.io/mkdocs-material/reference/admonitions/#supported-types).

### Onglets de contenu

Organisez plusieurs variantes d’un même contenu dans des onglets :

```markdown
=== "Windows"
    Instructions pour Windows...

=== "macOS"
    Instructions pour macOS...
```

=== "Windows"
    Instructions pour Windows...

=== "macOS"
    Instructions pour macOS...
