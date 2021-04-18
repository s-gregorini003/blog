# Quick Start

The theme I'm currently using is [Beautiful Hugo](https://github.com/halogenica/beautifulhugo).

## Install Hugo

1. Create a new folder `C:\Program Files\Hugo`.
2. Create a subfolder in the Hugo folder: `C:\Program Files\Hugo\bin`.
3. Create another subfolder `D:\Users\Silvio\dev\website`.
4. Download the latest zipped Hugo executable from the [Hugo Releases page](https://github.com/gohugoio/hugo/releases).
5. Extract all contents to your `..\Hugo\bin` folder.
6. Rename the executable (*hugo_hugo-version_platform_arch.exe*) to `hugo.exe` for ease of use.
7. In PowerShell (administrator), add the `hugo.exe` executable to your PATH by navigating to `C:\Program Files\Hugo\bin` and use the command `set PATH=%PATH%;C:\Program Files\Hugo\bin`. The `hugo` command should work after a reboot.

## Update Website

1. `git clone` this repo (**personal-website**) and **s-gregorini003.github.io** to `D:\Users\Silvio\dev\website`.
2. Update the website (create and edit posts, about, menus...) in `D:\Users\Silvio\dev\website`.
3. These are the commands to update the website:
```
cd D:\Users\Silvio\dev\website\personal-website
git status
git add .
git commit -m "COMMIT_MESSAGE"
git push origin master
hugo -d ../s-gregorini003.github.io/

cd ../s-gregorini003.github.io
git status
git add .
git commit -m "COMMIT-MESSAGE"
git push origin master

```
