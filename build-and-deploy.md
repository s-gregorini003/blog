---
title: "Build and Deploy"
date: 2021-04-18T15:14:44+01:00
draft: true
---

[link to the original article](https://bwaycer.github.io/hugo_tutorial.hugo/tutorials/github-pages-blog/)

# Project Site

This method works for [Project Sites](https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages#types-of-github-pages-sites), i.e. when the Hugo sources and the generated HTML output are stored in a single repository and the default domain of the website is `http(s)://<username>.github.io/<repository>`.

Reasons to use this method:
- It keeps sources and generated HTML in two different branches;
- It uses the default public folder;
- It keeps the histories of source branch and gh-pages branch fully separated from each other.

## Getting Started

These steps only need to be done once. First, add the public folder to .gitignore so it’s ignored on the master branch:

```sh
echo "public" >> .gitignore
```

Then, initialize the gh-pages branch as an empty [orphan branch](https://git-scm.com/docs/git-checkout/#git-checkout---orphanltnewbranchgt):

```sh
git checkout --orphan gh-pages
git reset --hard
git commit --allow-empty -m "Initializing gh-pages branch"
git push upstream gh-pages
git checkout master
```

## Building and Deployment

Now check out the gh-pages branch into your public folder, using git’s [worktree feature](https://git-scm.com/docs/git-worktree) (essentially, it allows you to have multiple branches of the same local repo to be checked out in different directories):

```sh
rm -rf public
git worktree add -B gh-pages public upstream/gh-pages
```

Regenerate the site using Hugo and commit the generated files on the gh-pages branch:

```sh
hugo
cd public && git add --all && git commit -m "Publishing to gh-pages" & cd ..
```

If the changes in your local gh-pages branch look alright, push them to the remote repo:

```sh
git push origin gh-pages
```

After a short while you’ll see the updated contents on your GitHub Pages site.


## Scripting

To automate these steps, you can create a script _scripts/publish_toghpages.sh with the following contents:

```sh
#!/bin/sh

DIR=$(dirname "$0")

cd $DIR/..

if [[ $(git status -s) ]]
then
    echo "The working directory is dirty. Please commit any pending changes."
    exit 1;
fi

echo "Deleting old publication"
rm -rf public
mkdir public
git worktree prune
rm -rf .git/worktrees/public/

echo "Checking out gh-pages branch into public"
git worktree add -B gh-pages public upstream/gh-pages

echo "Removing existing files"
rm -rf public/*

echo "Generating site"
hugo

echo "Updating gh-pages branch"
cd public && git add --all && git commit -m "Publishing to gh-pages (publish.sh)"
```

This will abort if there are pending changes in the working directory and also makes sure that all previously existing output files are removed. Adjust the script to taste, e.g. to include the final push to the remote repository if you don’t need to take a look at the gh-pages branch before pushing. Or adding `echo yourdomainname.com >> CNAME` if you set up for your gh-pages to use customize domain.