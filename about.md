---
layout: page
title: About
permalink: /about/
---

[nhathm](https://nhathm.com/)  
[techover.io](https://magz.techover.io/)

This is the base Jekyll theme. You can find out more info about customizing your Jekyll theme, as well as basic Jekyll usage documentation at [jekyllrb.com](https://jekyllrb.com/)

**Install Just The Docs guidelines**

Quick installtion:

- Check out project  
- Run command *bundle install* to install dependencies  
- Run command *bundle exec jekyll serve* to run local server

For some reason, Jekyll & bundler installer are not works, we need to complete installation

- Install ruby version 3:  
    `brew install chruby ruby-install xz`  
    `ruby-install ruby 3.1.3`

    `echo "source $(brew --prefix)/opt/chruby/share/chruby/chruby.sh" >> ~/.zshrc`  
    `echo "source $(brew --prefix)/opt/chruby/share/chruby/auto.sh" >> ~/.zshrc`  
    `echo "chruby ruby-3.1.3" >> ~/.zshrc # run 'chruby' to see actual version`

Restart terminal if need

- Install Jekyll  
    `gem install jekyll`  
    `bundle exec jekyll serve`
    or  
    `bundle exec jekyll serve --livereload` (for live reload when source code change)

- If still error:  
    Add webrick  
    `bundle add webrick`

- If still error:  
    Ask Google
