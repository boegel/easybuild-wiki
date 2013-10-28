Currently, GitHub doesn't allow assigning fine-grained permissions to repositories, so we can't allow people to easily contribute to the wiki pages without giving them full administrative access to the entire repository.

### Contribute

To contribute to the EasyBuild wiki, follow the steps below.

Note: steps 1 and 2 only have to be done **once**.

1. Fork the [`easybuild-wiki`](https://github.com/hpcugent/easybuild-wiki) repository that contains the raw wiki pages in MarkDown format
2. Clone your repository to your workstation, and define the `hpcugent` `easybuild-wiki` repository as upstream remote

```bash
git clone git@github.com:<GITHUB_LOGIN>/easybuild-wiki.git
cd easybuild-wiki
git remote add upstream git@github.com:hpcugent/easybuild-wiki.git
```
3. Edit a wiki page, commit the changes and push to your repository
```bash
vim Home.md
git commit -am "fix typo"
git push origin master
```
4. Create a pull request for your changes to the `hpcugent` repository

### Preview new wiki pages

To preview the changes you've just made, you can push them to the wiki of your own `easybuild-wiki` repository:

```bash
git remote add my_wiki git@github.com:<GITHUB_LOGIN>/easybuild-wiki.wiki.git  # only needed once
git pull my_wiki master
```

```bash
git push my_wiki master
```

You can then view your version of the wiki at https://github.com/<YOUR_GITHUB_LOGIN>/easybuild-wiki/wiki (note: change the `YOUR_GITHUB_LOGIN` part to obtain a valid URL).

### Update

To update your `easybuild-wiki` repository, use:

```
git pull upstream master
```

### Merging pull requests

**Note: the procedure below requires admin rights on the `easybuild` repository where the EasyBuild wiki is hosted, and thus might not work for you.**

When EasyBuild wiki contribution pull requests are merged in, the following steps should be performed to push the changes through to the actual wiki pages:

```bash
git remote add easybuild_wiki git@github.com:hpcugent/easybuild.wiki.git
cd easybuild-wiki
git pull origin master
git pull easybuild_wiki master
# resolve conflicts, if any
git push easybuild_wiki master
```