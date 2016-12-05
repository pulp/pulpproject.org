---
title: Contributing a Blog Post to pulpproject.org
author: Dennis Kliban
---
Quickstart Guide
----------------

1. Fork the pulpproject.org repository on GitHub.
   [https://github.com/pulp/pulpproject.org/](https://github.com/pulp/pulpproject.org/)

1. Clone your fork locally and checkout the `gh-pages` branch.

1. Create a new file in the `_posts` directory. The name for the file should be in the following
   format: `YYYY-MM-DD-title-of-post.md` e.g.
   `2016-11-30-contributing-blog-post-to-pulpproject.org.md`

1. Write a blog post in the new file.

1. Commit your changes to the `gh-pages` branch. By committing directly onto the `gh-pages` branch,
   you can preview it using Github.

1. Push your `gh-pages` branch to your fork.

1. Preview the blog at `https://<username>.github.io/pulpproject.org/blog/`. Another option to
   preview your blog post (without GitHub) is to [build the site
   locally](https://github.com/pulp/pulpproject.org/blob/gh-pages/README.md).

1. Create a Pull Request against the `gh-pages` branch of [pulproject.org
   repository](https://github.com/pulp/pulpproject.org/).

Writing the Blog Post
---------------------

Pulpproject.org is a [GitHub Pages](https://pages.github.com/) site. Each page is created using
[GitHub flavored Markdown](https://help.github.com/articles/basic-writing-and-formatting-syntax/).
Each blog post begins with the [YAML Front Matter](http://jekyllrb.com/docs/frontmatter/). Front
Matter specifies metadata such as the `author` and `title`. Here is an example of Front Matter for
a blog post:

    ---
    title: Django14 Retirement From EPEL 6
    author: Brian Bouterse
    tags:
      - platform
      - rpm
    ---

`tags` may be specified optionally, and the list of available tags is
[here](https://github.com/pulp/pulpproject.org/tree/gh-pages/tags). Including a tag or [multiple
tags](https://raw.githubusercontent.com/pulp/pulpproject.org/gh-pages/_posts/2016-11-17-django14-epel6-retirement.md)
makes the post discoverable by looking at posts organized by tag. More detailed documentation on
creating blog posts is available [here](http://jekyllrb.com/docs/posts/).

Previewing the HTML Site on GitHub
----------------------------------

Your fork of the pulpproject.org repository should already be configured to serve the `gh-pages`
branch at `http://<username>.github.io/pulpproject.org`. Once you push your changes to the
`gh-pages` branch, the blog can be viewed at `https://<username>.github.io/pulpproject.org/blog/`.
It may take a few minutes for the pages to be regenerated.

Previewing the HTML Site Locally
--------------------------------

Another option for previewing your blog entry is to build it locally. The simplest way to prepare
an environment for generating the Jekyll site is to use the Pulp development environment
provisioned using Vagrant. The documentation on provisioning using Vagrant can be found in the
[Pulp developer docs](http://docs.pulpproject.org/dev-guide/contributing/dev_setup.html#vagrant).
Once the Vagrant VM is provisioned, connect to it via ssh with the following commend:

`vagrant ssh -- -L 4000:127.0.0.1:4000`

The above command will forward your local port 4000 to port 4000 on the VM. This will enable you
to view the generated site at `http://127.0.0.1:4000`.

Gotchas
-------

Every time you push the `gh-pages` branch to GitHub, you will receive an email from GitHub
notifying you that the CNAME pulpproject.org is already taken. You can disregard these emails.

If the date in the name of the blog post file is in the future, the generated site will
not display the new blog post. While writing the post, you should use the current date in the
name of the file.
