
# Table of Contents

1.  [Introduction](#org46edbd1)
2.  [Implementation](#orgc5e2461)
3.  [Usage](#orgd09ee14)
    1.  [RSS Feeds](#org782aebd)
4.  [Features](#org6b85b25)
        1.  [Redirection](#org848f919)
    1.  [Drafts](#org1eba945)
5.  [License](#org0f304fe)



<a id="org46edbd1"></a>

# Introduction

This setup based on [org-mode](https://orgmode.org).


<a id="orgc5e2461"></a>

# Implementation

For details about the implementation see [the introductory post](posts/2019-09-03-migrating-from-jekyll-to-org/index.md) and the [\faGithub Source](https://github.com/dmacvicar/site.org).


<a id="orgd09ee14"></a>

# Usage

-   Posts go into `posts/`
    Preferably in their own subdirectory together with all their assets (self-contained)
-   Tutorials go in `tutorials/`, as they are rendered (for now) with the [readtheorg](https://github.com/fniessen/org-html-themes) theme

To generate:

    $ make

The output site will be rendered in `public/`.


<a id="org782aebd"></a>

## RSS Feeds

I am documenting this part because it is complicated and I tend to forget how it works everytime something breaks.

-   Each export block in the project translates the full site, file by file, to another format (taking into account excludes, includes)
    As we have very different settings for the posts than we have for tutorials, we have different entries in the project for translating each subset.

-   Sitemaps allow to generate a map of all converted files in a single org file.
    Eg. a sitemap in posts, will generate an org file with a list of links to all posts. This is particularly useful for the rss exporter.

-   We have an export entry in the project that goes over <span class="underline">\_posts/</span>, takes a dummy export function, but generates a sitemap. This allows us to end with a <span class="underline">rss.org</span> file without really exporting anything.

-   Then we use another entry in the project, that uses the <span class="underline">RSS</span> exporter, including only <span class="underline">rss.org</span> in order to generate <span class="underline">rss.xml</span>

-   Now, the default sitemap function generates something like:
    
        * Entries
          - [[One entry][http://link]]
          - [[Second entry][http://link]]
    
    Those will not be exported correctly to a list of links. Only the headline  will be exported in the RSS file.
    
        * [[One entry][http://link]]
        * [[Second entry][http://link]]

-   By default, the link will reference the file where the link appears. Something like
    
        <link>http://rss.org#section</link>
    
    To change this we use <span class="underline">org-rss-use-entry-url-as-guid</span> and set the <span class="underline">RSS<sub>PERMALINK</sub></span> property of the headline when generating the sitemap.
    The <span class="underline">RSS</span> exporter will then use the property for the <span class="underline"><link></span> tag content.

-   The custom entry formatter adds a headline to a temp buffer and then uses <span class="underline">org-set-property</span> to set the custom properties.
-   The sitemap function uses <span class="underline">org-list-to-generic</span> to create a list of headlines without adding a star, as it will be added by the entry function.


<a id="org6b85b25"></a>

# Features


<a id="org848f919"></a>

### Redirection

When the site was generated using Jekyll, posts where generated in the `/:year/:month/:day/:id.html` route.
In order to generate a redirect page, use the following keyword to generate a redirect page:

    #+REDIRECT_FROM /old/url/index.html


<a id="org1eba945"></a>

## Drafts

To hide a post from the list of recent posts or the full archive, use:

    #+DRAFT t

Note that the post will still be published, but you will need to access it by its URL directly.


<a id="org0f304fe"></a>

# License

The code used for generating the site is licensed under the [MIT](LICENSE) license.

The content of the site, Copyright (©) 2014-2019 Alexandre Mahrach.

