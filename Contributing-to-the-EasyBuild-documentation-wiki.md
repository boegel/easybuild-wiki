Currently, GitHub doesn't allow assigning fine-grained permissions to repositories, so we can't allow people to easily contribute to the wiki pages without giving them full administrative access to the entire repository.

### Contribute

To contribute to the EasyBuild wiki, follow the steps below.

Note: steps 1 and 2 only have to be done **once**.

1. Fork the [`easybuild-wiki`](https://github.com/hpcugent/easybuild-wiki) repository that contains the raw wiki pages in MarkDown format
2. Clone your repository to your workstation, and define the `hpcugent` `easybuild-wiki` repository as upstream remote

```
git clone git@github.com:<GITHUB_LOGIN>/easybuild-wiki.git
cd easybuild-wiki
git remote add upstream git@github.com:hpcugent/easybuild-wiki.git
```
3. Edit a wiki page, commit the changes and push to your repository
```
vim Home.md
git commit -am "fix typo"
git push origin master
```
4. Create a pull request for your changes to the `hpcugent` repository

### Update

To update your `easybuild-wiki` repository, use:

```
git pull upstream master
```