# Structuring content

---

## Markdown

This site is built with Material for MkDocs, which uses the Markdown language to write page content. Markdown is a lightweight markup language that allows you to format text in a simple, readable, and fast way, without needing to know HTML.  
This section is based directly on [:material-language-markdown-outline: *The Markdown Guide*](https://www.markdownguide.org/basic-syntax/).

Here are the main Markdown syntax rules to know:

### Headings

Markdown allows you to structure content using heading levels ranging from `#` (level 1) to `######` (level 6). Material for MkDocs automatically generates the table of contents from level 2 (`##`) and level 3 (`###`) headings.

```markdown
# Level 1 heading
## Level 2 heading
### Level 3 heading
```
Use only one level 1 heading (`#`) per page. It is generally used as the main title and is automatically used by MkDocs as the page title.

Use a logical heading hierarchy (`##`, `###`, etc.) so that the table of contents is clear and meaningful.

!!! tip "Compatibility"
    Markdown applications don’t all handle missing spaces between the number signs (#) and the heading text in the same way.
    For best compatibility:

    - Add a space between the `#` characters and the heading text.

        `## My title` :octicons-check-circle-fill-24:

        `##My title` :octicons-x-circle-fill-24:

    - Add a blank line before and after each heading. This ensures a stable rendering across all Markdown engines.

    ```markdown
    Add a blank line before...

    ## Heading

    ...and after each heading or block.
    ```

### Paragraphs

In Markdown, a paragraph is simply one or more lines of text separated by a blank line. By default, if you press ++enter++ without adding a blank line, the text will be merged into a single paragraph.

For example:

```markdown
Line 1  
Line 2
```

This will be displayed as a single continuous line, like:
Line 1 Line 2

However, it’s still useful to keep this formatting to make your file easier to read.

To insert a manual line break without creating a new paragraph, add two spaces at the end of the line:

```markdown
Line 1␣␣
Line 2
```

### Emphasis

Markdown lets you highlight text using **bold**, *italic*, and ***both at once***.

```markdown
*italic*
**bold**
***bold and italic***
```

You can also emphasize part of a word without spaces, like this:
`incred**ible**` → incredible

### Lists

Markdown allows you to create unordered (bulleted) or ordered (numbered) lists using simple syntax. To nest a sublist, indent the item with at least 2 spaces or a tab.

**Unordered list (bullets)**

To create a bulleted list, you can use the symbols `-`, `*` or `+` in front of each item. All three work, but it’s best to be consistent within a file.

```markdown
- Item 1
- Item 2
    - Nested item
```

**Ordered list (numbered)**

Use numbers followed by a dot (`1.`, `2.`, etc.) to create a numbered list. The numbers don’t have to be in order (Markdown will automatically reorder them), but the first item must start with `1.`.  
For best compatibility across Markdown engines, always use the dot `.` (and not a parenthesis) after the numbers.

```markdown
1. First
2. Second
    1. Nested item
3. Third
```
If you need to start an unordered list item with a number followed by a dot (e.g., "3.5 liters"), use a backslash (`\`) to prevent Markdown from treating it as a list item.

If you want stricter control over list behavior (especially nesting), you can enable the `sane_lists` extension in your `mkdocs.yml`:

```yaml title="mkdocs.yml"
markdown_extensions:
  - sane_lists
```

Markdown will no longer automatically renumber the list items.

### Links and Images

**Links**

In Markdown, you can easily insert clickable links using a simple syntax.
You can optionally add a title that will appear when hovering over the link. To do this, add it in quotes just after the URL.

```markdown
[Link text](https://example.com "This is an example")
```

There should be no space between `[` and `(` in links. Hovering over the link will display: *This is an example*.

!!! tip "Reference-style links"
    Markdown also allows you to define links in a more readable way by separating the link text from the URL.
    This is useful in long documents to avoid cluttering paragraphs with raw URLs.
    
    ```markdown
    [Documentation][material-mkdocs]

    [material-mkdocs]: https://squidfunk.github.io/mkdocs-material/
    ```

    You can place the reference (`[material-mkdocs]: ...`) anywhere in the file, typically at the bottom of the page.

If you want to quickly turn a raw URL or email address into a clickable link, just enclose it in angle brackets < >:

<address@example.com>

**Images**

Images work similarly:

```markdown
![Alt text](path/to/image.png)
```

### Quotes and Code

In Markdown, you can insert a quote or block of indented text using the `>` symbol at the beginning of the line.
You can also nest multiple levels of quotes.

```markdown
> This is a quote.
>> Nested quote.
```

> This is a quote.
>> Nested quote.

Material for MkDocs provides an alternative to standard Markdown quotes: informative blocks or callouts.
These help structure information visually using icons, colors, and customizable types (tip, warning, note, etc.).

!!! quote "Example"
    This is a quote.

This type of block is covered in a [dedicated section](#admonitions).

To format a word or phrase as inline code, wrap it in backticks `` ` `` (also known as grave accents).

```markdown
`inline code`
```

If the word or phrase already contains a backtick, you can escape it by using double backticks instead of a single one.

!!! info "What about code blocks?"
    To display multiple lines of code in a dedicated block, use multiline code blocks.
    These are fully supported by Material for MkDocs with advanced features like line numbering, copy buttons, syntax highlighting, etc.
    This is covered in the [dedicated section](#code-blocks).

---

## Material for MkDocs

In addition to standard Markdown, Material for MkDocs supports many enhanced Markdown extensions via `pymdown-extensions`, a powerful suite of tools designed to improve the readability, structure, and interactivity of your documentation.

These features are either built-in or can be easily activated via the `mkdocs.yml` configuration file under `markdown_extensions:`.

This section introduces the most useful extensions for making your documentation clearer, more readable, and more professional.

### Code blocks

Material for MkDocs supports interactive, styled code blocks with many options:

- Automatic syntax highlighting based on the language
- Line numbering
- “Copy” button in the top right corner of the block

To enable these options, add or edit the following sections in your `mkdocs.yml`:

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

The basic syntax is the same as standard Markdown: use three backticks (`` ``` ``) to open and close the block, and optionally add the language name to enable syntax highlighting.

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

Display a banner above the block with a title using `title="..."`.

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

You can also add `linenums="..."` to enable line numbering and `hl_lines="..."` to highlight one or more lines:

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

You can specify multiple lines or ranges:

````markdown
```py hl_lines="2 4 6-8"
````

### Admonitions

Admonitions can be used to highlight a note, warning, example, or important information.

To enable them, add or edit the following in your `mkdocs.yml`:

```yaml title="mkdocs.yml"
markdown_extensions:
  - admonition
  - pymdownx.details
  - pymdownx.superfences
```

Admonitions start with `!!!` followed by a keyword indicating the type.
You can also make them collapsible with `???` (collapsed) or `???+` (expanded).
By default, the title is based on the type, but you can customize it using quotes.


```markdown
!!! info
    This wiki uses Material for MkDocs.

??? warning "Caution"
    Be careful with filename casing.
```

!!! info
    This wiki uses Material for MkDocs.

??? warning "Caution"
    Be careful with filename casing.

It’s also possible to add a callout inline using `inline` or `inline end`.

!!! example inline end "Example"
    This type of callout takes up less vertical space.

```markdown
!!! example inline end "Example"
    This type of callout takes up less vertical space.
```

Available types include: `note`, `tip`, `quote`, `warning`, `danger`, `bug`, etc.
[See all available types and styles](https://squidfunk.github.io/mkdocs-material/reference/admonitions/#supported-types).

### Content Tabs

You can organize alternate versions of the same content using tabs:

```markdown
=== "Windows"
    Instructions for Windows...

=== "macOS"
    Instructions for macOS...
```

=== "Windows"
    Instructions for Windows...

=== "macOS"
    Instructions for macOS...
