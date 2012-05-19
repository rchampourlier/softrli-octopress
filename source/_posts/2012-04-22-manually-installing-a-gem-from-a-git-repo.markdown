---
layout: post
title: "Manually installing a gem from a git repo"
date: 2012-04-22 10:16
comments: true
categories: [development, ruby, notetomyself]

---
```
gem uninstall gem_name
git clone git://github.com/repo_name/gem_name.git
cd gem_name
gem build gem_name.gemspec
gem install ./<the gem that was compiled>
```