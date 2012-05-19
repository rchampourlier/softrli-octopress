---
layout: post
title: "Simple cheatsheet for git submodules"
date: 2012-04-14 10:01
comments: true
categories: [development, git, cheatsheet]

---
Just a cheatsheet for **git and submodules**.

<!-- more --> 

## Steps

1. Add the submodule using a `git submodule add...` command (see below for the different options).
2. Run `git submodule init`.

## Local submodules

```
git submodule add "/absolute/path/to/the/submodule/repository" local/path/for/the/submodule
```

To perform operations on the submodule, go in its folder. From then you can make commits and push them to the 'remote' repository.

## Remote submodules

Add a remote submodule, include external project and track updates

    git submodule add https://path_to_the_git_repository.git ./local_path_to_the_submodule_directory