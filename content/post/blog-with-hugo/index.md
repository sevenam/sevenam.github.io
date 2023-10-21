---
author: JB
title: Creating a blog with Hugo
date: 2023-10-21
description: Blogging in markdown with GoHugo
image: "logos/hugo-logo.jpg"
categories: [ "markdown" ]
tags: [ "markdown", "go", "hugo", "github" ]
---

Ref: https://gohugo.io

Hugo is a fast and modern static site generator written in Go, and designed to make website creation fun again. \
It is quite simple to install, set up and write content in using Markdown. \
I'll describe step by step how I set up and created the blog you are currently reading using Hugo and GitHub.

## Installing Hugo

The first thing to do is to install Hugo. See this link for detailed documentation about this for your OS:  https://gohugo.io/installation/

```bash
# windows
winget install Hugo.Hugo
# or extended version for SASS/SCSS support:
winget install Hugo.Hugo.Extended

# macos/linux
brew install hugo
```

## Setting up a new site

After installing the Hugo CLI the next step is to create a new site. \
See the following link for more detailed documentation: https://gohugo.io/getting-started/quick-start/

```bash

# documentation: https://stack.jimmycai.com/guide/getting-started
# github actions deployment: https://gohugo.io/hosting-and-deployment/hosting-on-github/

# create new site
hugo new site cognitiveoverload
```

## Picking a theme

The next step is to pick a theme for your site. With Hugo there are a lot of options to choose from. \
As I needed a theme for a blog, I picked the theme [Hugo Theme Stack](https://github.com/CaiJimmy/hugo-theme-stack/) as it had the features I was looking for. \
Big creds to [Jimmy Cai](https://github.com/CaiJimmy) for this theme! \
Check out some other themes here: https://themes.gohugo.io/

Start by adding the theme repo as a submodule:

```bash
# add theme as submodule
git submodule add https://github.com/CaiJimmy/hugo-theme-stack/ themes/hugo-theme-stack
```

After adding the submodule, there should be a theme in the `themes` folder in your new site. \
Then create a starting point for your content by copying the theme example into your own content folder:

```bash
# copy from:
# themes\hugo-theme-stack\exampleSite
# to the "content" folder
# (if you picked another theme, the theme folder will be different)
```

The next step is to add the theme to your Hugo config file. \
The config file should be a file called `hugo.toml` in the root directory of your new site. \
Because of own preferences I changed this file to a `hugo.yaml` file instead (just changing the file type does the trick).

```bash
# add theme to config in the "hugo.toml" or "hugo.yaml" file:
# the content of this variable probably has to match the themes/folder-name
theme = 'hugo-theme-stack'
```

There are a few things you can set up in the Hugo config file (and potentially theme specific things). \
For Hugo Theme Stack specific configs see example in: \
`themes/hugo-theme-stack/exampleSite/config.yaml` \
Here's an example of how the config file could look like:

```yaml
baseURL: 'https://cognitiveoverload.blog.com'
languageCode: 'en-us'
title: 'cognitive;overload'
theme: 'hugo-theme-stack'
paginate: 5

params:
    mainSections:
        - post
    featuredImageField: image
    rssFullContent: true
    favicon: /favicon-32x32.png

    widgets:
        homepage:
            - type: search
            - type: archives
              params:
                  limit: 5
            - type: categories
              params:
                  limit: 10
            - type: tag-cloud
              params:
                  limit: 10
        page:
            - type: toc

    sidebar:
        subtitle: A programming blog
        avatar:
            enabled: true
            local: true
            src: img/avatar.jpg
```

## Spinning up the site locally

To have a peek at how the site looks like, you can spin up web server locally with the Hugo CLI:

```bash
# serve with hugo locally on port 1313
hugo serve -p 1313
```

## Publishing on GitHub Pages

Ref: https://gohugo.io/hosting-and-deployment/hosting-on-github

Sign up for an account on https://github.com if you do not have one.
To publish to your github.io domain, make sure to create your site in a git repository that matches this naming strategy: `your-account-name.github.io` \
On github.com in your repository, go to "Settings" and then "Pages". Switch deployment from "Branch" to "Github Actions".

Create a new file in this directory in your repository: `.github/workflows/hugo.yaml`

Put this content in the file:

```yaml
# Sample workflow for building and deploying a Hugo site to GitHub Pages
name: Deploy Hugo site to Pages

on:
  # Runs on pushes targeting the default branch
  push:
    branches:
      - main

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Allow only one concurrent deployment, skipping runs queued between the run in-progress and latest queued.
# However, do NOT cancel in-progress runs as we want to allow these production deployments to complete.
concurrency:
  group: "pages"
  cancel-in-progress: false

# Default to bash
defaults:
  run:
    shell: bash

jobs:
  # Build job
  build:
    runs-on: ubuntu-latest
    env:
      HUGO_VERSION: 0.115.4
    steps:
      - name: Install Hugo CLI
        run: |
          wget -O ${{ runner.temp }}/hugo.deb https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.deb \
          && sudo dpkg -i ${{ runner.temp }}/hugo.deb          
      - name: Install Dart Sass
        run: sudo snap install dart-sass
      - name: Checkout
        uses: actions/checkout@v3
        with:
          submodules: recursive
          fetch-depth: 0
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v3
      - name: Install Node.js dependencies
        run: "[[ -f package-lock.json || -f npm-shrinkwrap.json ]] && npm ci || true"
      - name: Build with Hugo
        env:
          # For maximum backward compatibility with Hugo modules
          HUGO_ENVIRONMENT: production
          HUGO_ENV: production
        run: |
          hugo \
            --gc \
            --minify \
            --baseURL "${{ steps.pages.outputs.base_url }}/"          
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v1
        with:
          path: ./public

  # Deployment job
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v2
```

Pushing your code should now trigger a build of your new Hugo site. \
When the GitHub Action is complete, you should be able to view your site on your-account-name.github.io

## Adding disqus for comments

First sign up for disqus if you don't already have an account: https://disqus.com/profile/signup/ \
Then create a new disqus site and put the shortname of the site in your Hugo config:

```yaml
disqusShortname: shortname-of-your-disqus-site

params:
    comments:
        enabled: true
        provider: disqus
```


## Adding Google Analytics

Log in to https://analytics.google.com/ and create a new account for the site. \
Set up data collection in your account and you should get a Measurement Id. \
This Measurement Id needs to be added in your Hugo config like this: 

```yaml
googleAnalytics: G-YOURIDGOESHERE
```
