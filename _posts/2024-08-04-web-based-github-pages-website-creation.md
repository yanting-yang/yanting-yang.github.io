---
layout: post
title: "Web-based Github Pages Website Creation"
---

GitHub Pages is a free and easy service to host a website from a repository on GitHub.com. Various tutorials have be introduced on how to create a GitHub Pages website. However, most of previous tutorials require a development environment, which depends on the static site generator. These development environments are highly customizable and the compatability issues may occure due to the changing of software versions. Therefore, this post introduces a purely web-based method to create a GitHub Pages website, which does not need any local development. In this post, the default GitHub Pages workflow, which contains [jekyll-build-pages](https://github.com/actions/jekyll-build-pages) based on [pages-gem](https://github.com/github/pages-gem), is leveraged to build the website. The whole workflow is maintained by the GitHub Pages team so that the proposed method is more stable then self-customized one.

{% raw %} 

## Initialize a new site

1. Create a new repository

    Use `<user>.github.io` as the **Repository name**

    Go to **Settings** - **Pages**, under **Build and deployment** - **Branch**, you will be prompted that

    > GitHub Pages is currently disabled. You must first add content to your repository before you can publish a GitHub Pages site.

    Therefore, Github Pages can not be deployed on empty repository.

1. Create `README.md` in the repository root

    After committing the changes, a Github action named `pages build and deployment` will be automatically run to deply the site. You can then visit the `https://<user>.github.io/` to view your site.

    As stated in "[Static site generators](https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages#static-site-generators)" of the GitHub Docs:

    > If you publish your site from a source branch, GitHub Pages will use Jekyll to build your site by default.

    The software versions can be found in the action logs:

    ```text
    GitHub Pages: github-pages v231
    GitHub Pages: jekyll v3.9.5
           Theme: jekyll-theme-primer
    Theme source: /usr/local/bundle/gems/jekyll-theme-primer-0.6.0
    ```

    The default software versions that Github Pages use can be found in "[Dependency versions](https://pages.github.com/versions/)" of the GitHub Pages site.

    The default plugins can be found in "[Plugins](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/about-github-pages-and-jekyll#plugins)" of the GitHub Docs.

    The Github Pages is using the README as the site's index because the plugin [`jekyll-readme-index`](https://github.com/benbalter/jekyll-readme-index) is enabled.

## Add a theme to the site

1. Create `_config.yml` in the repository root
1. Add a new line to the file for the theme name

    For a list of supported themes, see "[Supported themes](https://pages.github.com/themes/)" on the GitHub Pages site.

    For example, to select the [Minima](https://github.com/jekyll/minima) theme, type `theme: minima`.

## Use a separate index file instead of the README

As the README is usually used to describe information of the rerpository and its content may differ from the index page of a website, we can let Github Pages to rander a `index.md` as the home page of the site.

1. Create `index.md` in the repository root
1. Add the following content:

    ```text
    ---
    layout: home
    ---
    ```

    The home page will then use the layout created by the theme.

## Enable Mathjax

1. Create `head.html` under `_includes`

    Add the following contents:

    ```html
    <head>
      <meta charset="utf-8">
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <meta name="viewport" content="width=device-width, initial-scale=1">
      {%- seo -%}
      <link rel="stylesheet" href="{{ "/assets/main.css" | relative_url }}">
      {%- feed_meta -%}
      {%- if jekyll.environment == 'production' and site.google_analytics -%}
        {%- include google-analytics.html -%}
      {%- endif -%}

      {%- include custom-head.html -%}

    </head>
    ```

    We need the create `head.html` as, for Minima v2.5.1, line `{%- include custom-head.html -%}` was not added so custom head can not be achieved.

1. Create `custom-head.html` under `_includes`

    Add `{% include mathjax.html %}` to include codes for Mathjax.

1. Create `mathjax.html` under `_includes`

    Add the following contents:

    ```html
    {% if page.mathjax %}
    <script>
    MathJax = {
      tex: {
        inlineMath: [
          ['$', '$']
        ]
      }
    };
    </script>
    <script id="MathJax-script" async
      src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js">
    </script>
    {% endif %}
    ```

1. To enable Mathjax for a post, add `mathjax: true` to the front matter.

{% endraw %}
