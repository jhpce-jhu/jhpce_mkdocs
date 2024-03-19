---
tags:
  - in-progress
---

# Tips and Conventions

We aim to create a great web site for our users. Consistency contributes to that result. Here are some conventions and time-saving tips.

PLEASE read through the [Features](../ourtools/features.md) page for ideas about ways to present information in the best manner. Materials for MkDocs has so many tools for creating attractive and useful pages!!!

## **Conventions/Checklist**
When editing pages on our site, please keep these things in mind.

### ***Update tags***
as needed to reflect current document status. Example `needs-to-be-written` becomes `in-progress` as the document is fleshed out and becomes somewhat useful to users.
### ***Remove authoring notes***
as their guidance is fulfilled by your modifying the document.
### ***Write information once, then refer to it***
Instead of placing versions of the same information in multiple places, put one authoritative version in the right document and then refer to it in other documents where needed. Example: In the FAQ, the answer to a question/issue may be a simple "See this document"
### ***Refer to specific locations within documents***
Jeffrey enabled "permalink" so each section of each document can have its own URL.
### ***Images live near their documents***
Each topic subdirectory under docs/ has an images/ directory to hold images for documents in that directory. This aids in web site maintenance, as it is more clear over time which of many dozen image files on the web site is used where.


## **Tips**

### ***Linking to documents within web site:***

* Because documents are divided up between directories by topic, any references you make to them need to use ***correct relative paths***.
* Links need to ***include the ".md"*** file name suffixes. These are not shown in the URL on the web site, but are required for links to be make correctly.
### Symbolic Link Image Files Which Change
We have some documents, such as Orientation PDF, which are updated. During the updates their file names often change in order to embed date versioning info.

Instead of embedding links in our web pages to the actual PDF file, and having to find and update all of the links every time the file name changes, Jeffrey has found that creating symbolic links with well-chosen static file names to the variable file names allows the links to remain constant.

```bash title="Make a target named latest-orient.pdf"
cd docs/orient/images
ln -s JHPCE-Overview-CMS-2023-12-2.pdf latest-orient.pdf
```

### ***Blank lines are required for some features to work:***
Some elements, such as lists like this one, rely on there being a blank line above the first item. Same is true for [admonitions](../ourtools/features.md/#admonitions) and [details](../ourtools/features.md/#details).
### ***Frontmatter Indentation:***
Items in YAML at the top of many pages **has to be indented** according to YAML rules, or things break.
### ***Indentation for block content:***
FOUR spaces is what you need to put in front of each paragraph you want to be included in something like an admonition or detail. FOUR, no more, no less. Four spaces is also needed to nest list items.
### ***Inspect example documents***
if you want to see how something was done in practice. The features page contains many such examples.
### ***Line numbers in code blocks***
They had been enabled by default until 20240310, when Jeffrey disabled them. When enabled, you got rid of them for a cod block by adding `linenums="0"` after the opening three backticks and a space, i.e. ++grave++ ++grave++ ++grave++ ++space++ linenums="0"

Now that they have been disabled, if you _want_ line numbering, you should _add_ `linenums="1"`

### ***Add a title, too***
to code blocks by adding `"title words"` after the opening three backticks and a space, i.e. ++grave++ ++grave++ ++grave++ ++space++ ++dblquote++ My title ++dblquote++ ++space++ linenums="0"

### 2 ways of embedding code blocks:
- With HTML:
<pre><code>[test@compute-107 ~]$ <span style="background-color:yellow">module list</span>

Currently Loaded Modules:
  1) JHPCE_ROCKY9_DEFAULT_ENV   2) JHPCE_tools/3.0
</code></pre>
- With Markdown:
``` shell-session
[test@compute-107 ~]$ module list 

Currently Loaded Modules:
  1) JHPCE_ROCKY9_DEFAULT_ENV   2) JHPCE_tools/3.0
```

