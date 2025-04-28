# Features we might/should implement

## Places with resources to investigate
This [best-of-mkdocs](https://github.com/mkdocs/catalog) is updated regularly and has categorized MkDocs plugins and other solutions generated from github star rankings.

This [page](https://chrieke.medium.com/the-best-mkdocs-plugins-and-customizations-fc820eb19759) has a number of recommendations that I find interesting. As always, one has to consider whether something is the best-in-class. The PDF export link below for example hasn't been updated in years. Is it still the right choice? Maybe.
Mermaid graph generation plugin, [PDF export](https://github.com/zhaoterryy/mkdocs-pdf-export-plugin), page redirects, exclude and exclude-from-search, Jupyter notebook

## Tables: Multi-line column headers
Maybe this is possible. Would be nice to be able to use more words-per-column without forcing the table to be pushed out and require the use of a scrollbar.

## Absolute paths
So references to other documents could be made to a single absolute path.
Right now you have to use relative paths, which is just prone to more errors. 

20240317 - JRT noticed that Adi's use of absolute paths for web portal images were rendered on the web site but not when viewed through a "mkdocs serve" session. He changed those links to be relative.

## Tables: Sortable tables
(Adi has enabled) (but there are other options we might want to enable)

They are possible. Might require an extension or plugin.

This might have been enabled by adding [this](https://squidfunk.github.io/mkdocs-material/reference/data-tables/#sortable-tables-mkdocsyml) to mkdocs.yml

Note that tablesort provides alternative comparison implementations like numbers, filesizes, dates and month names. See the [tablesort documentation](http://tristen.ca/tablesort/demo/) for more information.

## Announcement bar in header
For things like planned outages.
Update `/overrides/main.html` file and replace the message with any custom one. Site needs to be rebuild afterwards for the announcement to propagate to live.

To hide the announcement (or any overriden block) use `-%` at the top block definition and `%-` at definitioan closing.
```
{% block announce -%}
{%- endblock %}
```
To adjust colors modify `docs/stylesheets/extra.css` and adjust the `.md-banner` style definition to match your color preference.
```
.md-banner {
  background-color: #ffcc00;
  color: #cc3300;
}
```
## Open URLs in new tabs
(Adi has enabled)
I think there might be plugins which make this easier than what you have to do othwerwise, which is to use HTML instead of the Markdown notation. In normal HTML you add a space and a string to the end of the URL: `target="_blank"`

```
<a href="https://squidfunk.github.io/mkdocs-material/reference/admonitions/" target="_blank">About admonitions</a>
```

## Maybe a user-contribution capability
For topic headings like SOFTWARE where users have recipes and tuning advice. Implemented how? git pull requests against any page on the site? Is there a way to limit to a subsection?

## Search enhancements
```
plugins:
  - search
```
  
Search is a plugin.
There are many plugins out there for MkDocs. A different set, I suppose, for Material for MkDocs. Some overlap, some exclusive.

Note that search is enabled by default but if you add options it HAS TO BE LISTED in the plugins stanza.

There are A LOT of extras you can do. Someone explore later if desired.

https://squidfunk.github.io/mkdocs-material/setup/setting-up-site-search/

## BLOG
A blog writable only by site administrators. As a tool for making announcements.
```
plugins:
  - blog
```
Blog is a plugin.
https://squidfunk.github.io/mkdocs-material/setup/setting-up-a-blog/
Be aware that you cannot use features preceeded by "Insiders"
This is enabled by adding a line to the plugins stanza

`- blog`

## last-modified dates on documents
(Adi added this. Jeffrey added the info to the "mkdocs serve" recipe in features.md)
This might be provided by multiple plugins out in the world.
The one that might be the most common is "git-revision-date-localized"

## Glossary
There's a way to create a document which is automatically updated when people define [abbreviations](features.md#abbreviations).
Jeffrey created a glossary by hand.

