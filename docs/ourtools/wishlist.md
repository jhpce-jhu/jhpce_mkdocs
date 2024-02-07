# Features we might/should implement

## Multi-line column headers
Maybe this is possible. Would be nice to be able to use more words-per-column without forcing the table to be pushed out and require the use of a scrollbar.

## Absolute paths
So references to other documents could be made to a single absolute path.
Right now you have to use relative paths, which is just prone to more errors. 

## Sortable tables
They are possible.

## Announcement bar in header
For things like planned outages.
See issue 777.

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
This might be provided by multiple plugins out in the world.
The one that might be the most common is "git-revision-date-localized"

## Glossary
There's a way to create a document which is automatically updated when people define [abbrieviations](features.md#abbrieviations).
