# A repo for the JHPCE documentation web site 

## End result
+ The public URL is [https://jhpce.jhu.edu/](https://jhpce.jhu.edu/)
+ The live version of the site is at: [https://jhpce-jhu.github.io/jhpce_mkdocs/index.html](https://jhpce-jhu.github.io/jhpce_mkdocs/index.html)

## Relationship between Markdown, MkDocs, and Materials for MkDocs
In short: you write Markdown documents and MkDocs turns them into a static web site using the Material for MkDocs theme. 

* `Markdown` is the lightweight markup language used to write the content (headings, lists, code blocks, etc.).
* `MkDocs` is a Python-based static site generator that converts those Markdown files into a documentation website.
* `Material for MkDocs` is a ***theme and feature framework*** built on top of MkDocs — it layers "Material Design" styling, client-side search, dark mode, code annotations, content tabs, admonitions, and 10,000+ icons/emojis onto the base `MkDocs` build pipeline. 

Material for MkDocs is the most-installed third-party MkDocs theme on PyPI. It makes sites look and behave professionally, without any custom CSS or JS (although you can override things with custom CSS (and we do a little of that).

## Key documentation
+ [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/)
+ [Mkdocs documentation](https://www.mkdocs.org/getting-started/)
+ [Materials for MkDocs for site administrators](https://jhpce.jhu.edu/ourtools/features/) - precious info
+ [JHPCE Tips and Conventions](../ourtools/tips-conventions.md) - JHPCE-specific authoring guidance and tips.

## Vital operational things for systems administrators to know

1. You must include the string `rebuild-site` in the Git commit comment to cause the web site to be re-generated.
2. You can check on the status of rebuilding the web site in the [Workflows section](https://github.com/jhpce-jhu/jhpce_mkdocs/actions/workflows/ci.yml).

## Some basics of site operation

In short: you write Markdown documents, add them to this repo, and the `rebuild-site` string in the commit comment causes a GitHub Action to run. A custom YAML file contains the program needed to use `MkDocs` to rebuild the site.

+ The `docs` directory: contains Markdown files used to generate the site. Add or edit files inside there. 
+ The `mkdocs.yml` file defines the web site. It contains directives to enable features and a navigation bar to present the site structure.
+ Github Actions builds the site fromo `docs` into the `gh-pages` branch.
+ `mkdocs build` should run without errors or warnings.
+ Images files should be placed in `docs/*/images` directories (where `*` = topics (headings)
+ Consider placing other non-Markdown files files into `docs/*/images` directories to keep document directories "cleaner".


