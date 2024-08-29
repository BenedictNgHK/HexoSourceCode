---
title: Manage Hexo with Git
author: Benedict Ng
categories:
    - Hexo
---
It's annoying to manage Hexo blogs with git and Github because of lacking of resources in internet. Here I show a method to manage my Hexo blog.

The repos in Github will be:

1. `username.github.io` repo. This is the Github page repo to demonstrate your blog.
2. `hexo-theme` repo. This repo is to store your hexo theme. It is forked from other authors.
3. `HexoSourceCode` repo. This is the repo to store your source code of your Hexo project and it contains a submodule used as theme.

## Setup deployment configuration

This is already specified in the official website. So I just attach the link below. Please note: the url must match with your Github page repo.

[How to deploy](https://hexo.io/docs/github-pages#One-command-deployment)

## Fork a desired theme and clone it

1. Go to your desired theme Github repo and fork it as your own. You can rename your forked repo..
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

## Create source code repo in Github and associate it with local repo

Just associate your local repo with your `HexoSourceCode` repo like normal.

1. Create a remote repo used to store your source code in Github. Let's assume its name is `HexoSourceCode`.
2. Initialize your local git repo in your Hexo folder.

```shell
git init
```

3. Add all files to staged zone and commit them.

```shell
git add --all
git commit -m "Initial Commit"
```

4. Associate you local repo with remote repo.

```shell
git remote add origin HexoSourceCode_url.git
```

5. Push to remote repo

```shell
git push -u origin main
```