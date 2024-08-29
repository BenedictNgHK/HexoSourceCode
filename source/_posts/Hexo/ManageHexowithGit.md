---
title: Manage Hexo with Git
author: Benedict Ng
categories:
    - Hexo
---
It's annoying to manage Hexo blogs with git and github because of lacking of resources in internet. Here I show a method to manage my Hexo blog.

Before getting started, let's assume you have done the following works:

1. You have setup your github page and the local blog can be deployed to it.
2. You have initialized a you Hexo project folder as git repo and linked it with the remote github page repo.

Note: You can push the source code of the Hexo project to your github page repo and the github page still can be deployed properly.

The result will be:

1. `username.github.io` repo. It is to store the project source files and used as github pages, and it contains a submodule used as your theme.
2. `HexoSourceCode` repo.
3. `hexo-theme` repo. This repo is to store your hexo theme. It is forked from other authors.

## Fork a desired theme and clone it

1. Go to your desired theme github repo and fork it as your own. You can rename your forked repo..
2. Go to your forked repo and clone it to the `themes` folder. The command should be:

```shell
## Hexo
git clone forked_repo_url.git ./themes/your_theme_name
```

## Add your forked repo as submodule

1. Add your theme as submodule

```shell
git submodule add forked_repo_url.git /themes/your_theme_name
```

2. Update and init submodule

```shell
git submodule init
git submodule update
```

3. Stage and commit the changes

```shell
git add .gitmodules themes/your_theme_name
git commit -m "Added submodule from existing repository"
```

4. Check status

```shell
git status
```

And now you can commit your changes and push them to your remote repo.
