# WEB SITE TOOLS/ENABLED FEATURES WORTH KNOWING HOW TO USE
Some more useful than others. Jeffrey likes "admonitions" a lot. There is another document containing [wishlist](wishlist.md) items that we might want to enable/configure.

## Critical reference
[Materials for MkDocs reference section](https://squidfunk.github.io/mkdocs-material/reference/).

## Frontmatter (in documents)
Tags are the primary use of frontmatter I think we should use at this point. [This](https://squidfunk.github.io/mkdocs-material/reference/) may not be a complete list of directives that one can optionally add within a document. But the basics are that you can add to the top of the document a stanza to set the title of the document, a description of it, a status indicator such as new or deprecated. See [this page](https://squidfunk.github.io/mkdocs-material/reference/#setting-the-page-icon) for how to define an icon for the page.


## Tags
An example of frontmatter is the code to [add tags](https://squidfunk.github.io/mkdocs-material/setup/setting-up-tags/) to documents. 20240211 I tested adding a tag and it works. I also specified in the nav section [a page ](tags.md)for Material for MkDocs to automatically list tags and the pages they are found on.

A lot of optional tag-related settings/capabilities seem to be reserved for paying sponsers as of 20240201. See this [page](https://squidfunk.github.io/mkdocs-material/insiders/). Can users search for documents by tags in the search field?

The tags I envision using at the outset are shown below, so we can try to use them to figure out which pages need attention and possibly who is assigned to finish it.

The tag "needs-improvement-later" could be added to a page which has "done" to indicate that what we have is able to be published but needs more work.

The tag "contains-refs-to-old-site" came to me because some pages, such as the R page, contain screenshots which contain the URL of the old web site.

The tag "last-revised-YYYYMMDD" could be used on a page which also has "done" or "needs-improvement-later" so you can tell that information by looking at the tags page. As opposed to having to go look at the repository. The [wishlist](wishlist.md) document mentions adding a plugin or extension which allows the last-revised date to be automatically generated and listed at the bottom of each page.

Place lines like this at the very top of the document, before the document title, to add the tags mentioned. Tags are strings but I am hoping to avoid spaces or underscores. (Underscores suck b/c they require the shift key. And you can't always see them depenind on how text renders.)

```
---
tags:
  - done
  - needs-review
  - in-progress
  - needs-improvement-later
  - contains-refs-to-old-site
  - last-revised-20240210
  - jeffrey
  - mark
  - jiong
  - adi
  - brian
---
```

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

<a href="https://squidfunk.github.io/mkdocs-material/reference/admonitions/"" target="_blank">About admonitions</a>

You add an admonition by

1. starting a line with three explanation marks, a space, and a keyword (called a "type qualifier") such as note, danger, example, info, tip, warning. Here is a [list](https://squidfunk.github.io/mkdocs-material/reference/admonitions/#supported-types)
2. on the next line(s) start with FOUR spaces

!!! note
    Some text in a note.

!!! example
    admonitions allow setting off info inside colored boxes, e.g. note,tip,warning,danger,example.
    https://squidfunk.github.io/mkdocs-material/reference/admonitions/#usage

## Open URLs in new tabs
I think there might be plugins which make this easier than what you have to do othwerwise, which is to use HTML instead of the Markdown notation. In normal HTML you add a space and a string to the end of the URL: `target="_blank"`

```
<a href="https://squidfunk.github.io/mkdocs-material/reference/admonitions/" target="_blank">About admonitions</a>
```
    
## Footnotes
This[^1] is a reference to the feature's description.

You add a footnote by entering

`[^1]`

in the midst of your text. Anywhere in the document you write the footnote by placing at the start of a line the corresponding numbered entry using the same syntax but adding a colon and a space character after the closing square brace.


`[^1]: Wording of footnote`


[^1]: https://squidfunk.github.io/mkdocs-material/reference/footnotes/#adding-footnote-references


## Data tables
They don't render in MacDown (and maybe others) unless you also include the line of hyphens under the line containing the column titles.

You can align the column contents to left, center or right by placing a colon at the left side of the divider for that column, both sides, or the right side.

### Sortable tables
This is now implemented.

https://squidfunk.github.io/mkdocs-material/reference/data-tables/#sortable-tables


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

!!! note:
    The language keywords are not the same as in github -- there is overlap but also differences. See this list for the  [language keywords](https://pygments.org/docs/lexers/) for the Pygments
Python syntax highlighter used by Material for MkDocs.

Earlier text:

Like in Github you can specify a [programming language keyword](https://github.com/github-linguist/linguist/blob/master/lib/linguist/languages.yml) immediately following the leading three backticks to cause the text to be formatted in that language's notation. 

There is a directory [full of examples](https://github.com/github-linguist/linguist/tree/master/samples) you can simply click on.
 
Useful keywords include Awk, checksums, DNS Zone, Jupyter Notebook, Python, R, Regular Expression, Rich Text Format, SAS, Shell, sed, SSH Config/filenames, stata, YAML

A python example

```python
def fn():
pass
```
## Internal links
From [this mkdocs](https://www.mkdocs.org/user-guide/writing-your-docs/#writing-with-markdown) page JRT learned that you can specify anchor points to document sections knowing that they are converted to lowercase and white space is replaced by dashes. So this very section, named "Internal links" can be specified as a link to "features.md#internal-links"
