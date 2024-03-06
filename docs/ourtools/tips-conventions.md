---
tags:
  - in-progress
---

# Tips and Conventions

We aim to create a great web site for our users. Consistency contributes to that result. Here are some conventions and time-saving tips.

PLEASE read through the [Features](../ourtools/features.md) page for ideas about ways to present information in the best manner. Materials for MkDocs has so many tools for creating attractive and useful pages!!!

## Conventions/Checklist
When editing pages on our site, please keep these things in mind.

* ***Update tags*** as needed to reflect current document status. Example `needs-to-be-written` becomes `in-progress` as the document is fleshed out and becomes somewhat useful to users.
* ***Remove authoring notes*** as their guidance is fulfilled by your modifying the document.
* ***Write information once, then refer to it*** Instead of placing versions of the same information in multiple places, put one authoritative version in the right document and then refer to it in other documents where needed. Example: In the FAQ, the answer to a question/issue may be a simple "See this document"
* ***Refer to specific locations within documents*** Jeffrey enabled "permalink" so each section of each document can have its own URL.


## Tips

* ***Linking to documents within web site:***
    * Because documents are divided up between directories by topic, any references you make to them need to use ***correct relative paths***.
    * Links need to ***include the ".md"*** file name suffixes. These are not shown in the URL on the web site, but are required for links to be make correctly.
* ***Blank lines are required for some features to work:*** Some elements, such as lists like this one, rely on there being a blank line above the first item. Same is true for [admonitions](../ourtools/features.md/#admonitions) and [details](../ourtools/features.md/#details).
* ***Frontmatter Indentation:*** Items in YAML at the top of many pages **has to be indented** according to YAML rules, or things break.
* ***Indentation for block content:*** FOUR spaces is what you need to put in front of each paragraph you want to be included in something like an admonition or detail. FOUR, no more, no less. Four spaces is also needed to nest list items.
* ***Inspect example documents*** if you want to see how something was done in practice. The features page contains many such examples.
