# WEB SITE TOOLS/ENABLED FEATURES JEFFREY IS ACCUMULATING
Some more useful than others. I like "admonitions" a lot.

## Critical reference
[Materials for MkDocs reference section](https://squidfunk.github.io/mkdocs-material/reference/).

## Frontmatter (in documents)
I don't think that this is a complete list of directives that one can optionally add within a document.
https://squidfunk.github.io/mkdocs-material/reference/

## Abbreviations
Abbreviations can be defined by using a syntax similar to URLs and footnotes, starting with an asterisk immediately followed by the term to be associated in square brackets.

This code creates the following sentence. You can hover over "HTML" and see the definitino appear.
```
The HTML specification is maintained by the W3C.
*[HTML]: Hyper Text Markup Language
```

The HTML specification is maintained by the W3C.
*[HTML]: Hyper Text Markup Language

## Glossary
There's a way to create a document which is automatically updated when people define abbrieviations. See the wishlist document for details.

## definition list
You can create an indented block of text using a colon followed by FOUR space characters.

Example code and result:
```
`a sample term to define`
:    The definition you are seeking. (But not the droids.)
```

`a sample term to define`
:    The definition you are seeking. (But not the droids.)

## admonitions
These are **sweet**! We should use them frequently. But be aware that they are not rendered correctly in MacDown.app.

[About admonitions](https://squidfunk.github.io/mkdocs-material/reference/admonitions/)

!!! note
    Some text in a note.

!!! example
    admonitions allow setting off info inside colored boxes, e.g. note,tip,warning,danger,example.
    https://squidfunk.github.io/mkdocs-material/reference/admonitions/#usage
    
    
    
## Footnotes
This[^1] is a reference to the feature's description. 
[^1]: https://squidfunk.github.io/mkdocs-material/reference/footnotes/#adding-footnote-references


## Tags
Not attempted at this point. Optional feature allowing users to search by tags.
https://squidfunk.github.io/mkdocs-material/setup/setting-up-tags/
A lot of optional tag-related settings/capabilities seem to be reserved for paying sponsers as of 20240201. See this [page](https://squidfunk.github.io/mkdocs-material/insiders/).

## Data tables
They don't render in MacDown (and maybe others) unless you also include the line of hyphens under the line containing the column titles.

You can align the column contents to left, center or right by placing a colon at the left side of the divider for that column, both sides, or the right side.

### Sortable tables
JRT thinks that you need to add some javascript to enable sortable tables and other to enable instant loading. I suppose as opposed to having to manually reload the page??

https://squidfunk.github.io/mkdocs-material/reference/data-tables/#sortable-tables

http://tristen.ca/tablesort/demo/

### Import CSV or Excel file
See https://timvink.github.io/mkdocs-table-reader-plugin/

## ICONS and EMOJOIS: Examples: Kinds of check marks
JRT found that these three kinds of check marks are examples of "icons" and "emojis".

|Example|Markdown text|
|-------|-------------|
|:material-check:|`:material-check:`|
|:material-close:|`:material-close:`|
|:material-check-all:|`:material-check-all:`|

They would not render until I added these three lines to mkdocs.yml:
```
  - attr_list
  - pymdownx.emoji:
      emoji_index: !!python/name:material.extensions.emoji.twemoji
      emoji_generator: !!python/name:material.extensions.emoji.to_svg
```

See this page for more information and links to the "icon sets" as well as other pages that display a bazillion emojis:
https://squidfunk.github.io/mkdocs-material/reference/icons-emojis/


:smile: works if you enter `:smile:`

But these other things didn't work. For some of them I downloaded an .svg file for them and placed them in both a top-level .icons/ directory and a docs/.icons/ directory. So read more if you want to be able to put all kinds of cool symbols in our pages. DO YOU NEED TO HAVE .ICONS DIRECTORIES AT ALL?

:page-facing-up:

:mdiDatabaseSettingsOutline:

:mdiOrderAlphabeticalAscending:

:fa-regular fa-envelope:

## Fenced code blocks

Note that you can set off a block of text using three preceding and three following backtick characters. 

Like in Github you can specify a [programming language keyword](https://github.com/github-linguist/linguist/blob/master/lib/linguist/languages.yml) immediately following the leading three backticks to cause the text to be formatted in that language's nottation. 

There is a directory [full of examples](https://github.com/github-linguist/linguist/tree/master/samples) you can simply click on.
 
Useful keywords include Awk, checksums, DNS Zone, Jupyter Notebook, Python, R, Regular Expression, Rich Text Format, SAS, Shell, sed, SSH Config/filenames, stata, YAML

A python example

```python
def fn():
pass
```
## Internal links
From [this mkdocs](https://www.mkdocs.org/user-guide/writing-your-docs/#writing-with-markdown) page JRT learned that you can specify anchor points to document sections knowing that they are converted to lowercase and white space is replaced by dashes. So this very section, named "Internal links" can be specified as a link to "features.md#internal-links"
