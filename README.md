ai-labs.org blog
================

ai-labs.org blogging with Jekyll

Dependencies & Installing
-------------------------

    $ sudo aptitude install ruby ruby-dev rubygems1.9.1 nodejs nodejs-dev
    $ gem install jekyll

Usage
-----

1. $ cd ai-labs.github.io
2. $ jekyll build // The current folder will be generated into ./_site
3. $ jekyll serve // A development server will run at http://localhost:4000/

Drafts
------

To get local drafts work, 
follow [instructions](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/) 
to configure local environment.

For drafts you should create `_drafts` directory and put your markdown page
into this directory. Then run jekyll with `$ jekyll serve --drafts` command.


