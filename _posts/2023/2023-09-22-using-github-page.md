---
layout: post
title: Using Github Page
date: 2023-09-22 11:03 +0800
categories: [Gleanings, Technical Supports] 
tags: [Github Page]
math: true
---

# Using Github Page to Make Personal Blog

## Register Github Page

- To use github page to make personal blog, firstly register at github page, following the instructions from offical pages:
  - <https://pages.github.com/>
  - This instruction includes the minimal steps to deploy a blank ``hello world`` page on github page:
    - First create a public repo named ``username.github.io``
    - Then clone it to local and echo ``hello world`` to ``index.html``
        ```bash
        git clone https://github.com/username/username.github.io
        cd username.github.io
        echo "Hello World" > index.html
        ```
    - Finally add commit then push to github server, and the page would be deployed on ``https://username.github.io``
        ```bash
        git add -A 
        git commit -m "initial commit"
        git push -u origin main
        ```

## Offical Documentations of Github Page:

- For detailed documentation and instructions, see the offical documents:
  - <https://docs.github.com/en/pages>

## Using Jekyll Theme

- The theme used here is a jekyll theme ``Chirpy``: <https://github.com/cotes2020/jekyll-theme-chirpy>
  - For instructions, see <https://chirpy.cotes.page/>
- For quick reference, the most basic steps are listed below:
  - To give a quick start-up, one could use the [Chirp Starter](https://github.com/cotes2020/chirpy-starter) template. In Chirp Starter's repo, click the button ``Use this template`` > ``Create a new repository``, then rename the newly created repo as ``username.github.io`` where replace the ``username`` as your github username.
  - Install ``jekyll`` following [the offical site of jekyll](https://jekyllrb.com/)
  - Then run 
    ```bash
    $ bundle
    ```
    under the root directory **of your website** to install dependencies **before** running local server for the first time.
  - Configure the ``_config.yml`` file into your information
    - Set ``timezone`` to ``Asia/Shanghai`` 
    - Set ``url`` to ``https://username.github.io``
        > It is important to set `https://` into ``url``, or errors would be yielded when runing github auto-deployment
        {: .prompt-warning}
    - Set ``avatar`` to ``/commons/avatar.png`` , create ``/commons/`` directory under the root directory of your website if needed. Then put your avatar pitcure in it.
  - To deploy:
    -  In your website's github repo, <kbd>settings</kbd> > <kbd>Pages</kbd> .Then, in the **Source** section (under Build and deployment), select GitHub Actions from the dropdown menu.
    ![github action](../../commons/img/2023/Screenshot%202023-09-22%20131811.png)
  - Changing the favicon
    - First create the favicon by the website <https://realfavicongenerator.net/>, click the button <kbd>Select your Favicon</kbd> image to upload your image file. In the next step, the webpage will show all usage scenarios. You can keep the default options, scroll to the bottom of the page, and click the button <kbd>Generate your Favicons and HTML code</kbd> to generate the favicon.
    - Then download the generated package, unzip and delete the following two from the extracted files:
      - `browserconfig.xml`{: .filepath}
      - `site.webmanifest`{: .filepath}
    - And then copy the remaining image files (`.PNG`{: .filepath} and `.ICO`{: .filepath}) to cover the original files in the directory `assets/img/favicons/`{: .filepath} of your Jekyll site. If your Jekyll site doesn't have this directory yet, just create one.
    - Note to change the file ``android-chrome-384x384.pnd`` to ``android-chrome-512x512.pnd`` if the file you get is the former one. Also, copy the following files from the theme's github repo <https://github.com/cotes2020/jekyll-theme-chirpy> if you don't have them under ``/assets``
      - `browserconfig.xml`{: .filepath}
      - `site.webmanifest`{: .filepath}

    - The following table will help you understand the changes to the favicon files:

      | File(s)             | From Online Tool                  | From Chirpy |
      |---------------------|:---------------------------------:|:-----------:|
      | `*.PNG`             | ✓                                 | ✗           |
      | `*.ICO`             | ✓                                 | ✗           |

      >  ✓ means keep, ✗ means delete.
      {: .prompt-info }

    - Then the next time you build the site, the favicon will be replaced with a customized edition.
  - Writing new post:
    - It is more convenient to create new posts with [jekyll-compose](https://github.com/jekyll/jekyll-compose) which automatically generate the Front Matter of the post
    - To install it, see the instructions from the github repo <https://github.com/jekyll/jekyll-compose#installation>:
      - add the following line to the Gemfile:
        ``gem 'jekyll-compose', group: [:jekyll_plugins]``
      - Then execute
        ```console
        $ bundle
        ```
    - Then configure the ``_config.yml`` file as
      - ```yml
        # configure the jekyll-compose features
        jekyll_compose:
          default_front_matter:
            drafts:
              categories:
              tags:
              math: true
            posts:
              categories:
              tags:
              math : true
        ```
    - and to create new post, run
      - ```console
        $ bundle exec jekyll compose "My New Post" --post
        ```
      - This would create a new file named ``YY-MM-DD-my-new-post`` under `_posts/` with the front matter like
      
        ```markdown
        ---
        layout: post
        title: My New Post
        date: 2023-09-22 13:54 +0800
        categories: 
        tags: 
        math: true
        ---
        ```
      Then you can specify the categories and tags of this post, by assigning a list, like:

        ```markdown
        categories: [category1,category2]
        tags: [tag1,tag2]
        ```
    - For more information, see <https://chirpy.cotes.page/posts/write-a-new-post/>
  - Finally, push git commit to your github repo, and the website would be automatically deployed.



























