---
title: "Downsides of Arch Linux"
date: 2024-12-19
categories: website
---
As you know Arch Linux is a rolling release, so there are constant updates. This blog depends on uses the Jekyll framework and everytime I update ruby, website fails to build. Like usual, it is a good practice to use virtual environments when building projects that have strict dependencies, just like pyenv with Python. Now I am using ruby version 3.2.6 for this blog.

To do this:
1. Install `rbenv` from AUR
2. Install `ruby-build` from AUR
3. Add `eval "$(rbenv init -)"` to bashrc and source .bashrc
4. Run `rbenv install 3.2.6`
5. In the root project dir, execute `rerbenv local 3.2.6`
6. `gem install jekyll bundler`, `bundle install`, `bundle exec jekyll serve`

