# Creating a TOC for MD files

Some tools and applications offer automatically creating a table of contents based on headlines defined in the markdown files. To name one example: [Gitlab](https://gitlab.com/gitlab-org/gitlab-foss/-/issues/2494) supports it in wikis and non-wiki areas.

## Tool-driven creation of ToC

For Windows, Mac OS etc. you can use [github-markdown-toc](https://github.com/ekalinin/github-markdown-toc.go) to create the table for you.

Add the path to the downloaded executable to the PATH variable (depending on your OS) to use the application without having to add the full path to it.

## Automating the process

As I don't have a script available right now, here are the basic steps to perform:

1. Read all MD files of a directory.
2. Scan each file for the term `[[TOC]]`.
   1. If it exists, run `gh-md-toc FILENAME` and buffer the result.
   2. Replace the ``[[TOC]]`` with the result.
   3. Save changes in original file.