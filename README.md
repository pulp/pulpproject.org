## pulpproject.org

This repo holds content for pulpproject.org. Any changes merged to the `gh-pages` branch will be
available within a few minutes on https://pulpproject.org/

## Building Locally

1. Clone this repo and checkout the `gh-pages` branch
2. Install "bundler" with: `gem install bundler`
3. Use bundler to install Jekyll and gh-pages. In the top level of your check run: `bundle install`

## Browse the Local Site
1. Serve the site locally so you can test your changes: `bundle exec jekyll serve`
2. View your docs locally at: `http://localhost:4000`

## Using with Vagrant

If your `bundle exec jekyll serve` is run from inside a Vagrant machine you can browse the docs
from your host system by connecting to Vagrant via ssh with a tunnel for port 4000. For example:

`vagrant ssh -- -L 4000:127.0.0.1:4000`

## Updating Dependencies

Updating the dependencies of a build environment is done with: `bundle update github-pages`

