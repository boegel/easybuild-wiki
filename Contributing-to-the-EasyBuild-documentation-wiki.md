Currently, GitHub doesn't allow assigning fine-grained permissions to repositories, so we can't allow people to easily contribute to the wiki pages without giving them full administrative access to the entire repository.

### Contribute

To contribute to the EasyBuild wiki, follow the steps below.

#### Setup (only needed once)

* Fork the [`easybuild-wiki`](https://github.com/hpcugent/easybuild-wiki) repository that contains the raw wiki pages in MarkDown format
* Clone your repository to your workstation, and define the `hpcugent` `easybuild-wiki` repository as upstream remote
```bash
git clone git@github.com:YOUR_GITHUB_LOGIN/easybuild-wiki.git
cd easybuild-wiki
git remote add upstream git@github.com:hpcugent/easybuild-wiki.git
```

#### Make a contribution

* Edit a wiki page, commit the changes and push to your repository
```bash
vim Home.md
git commit -am "fix typo"
git push origin master
```
* Create a pull request for your changes to the `hpcugent` repository

### Preview new wiki pages

To preview the changes you've just made, you can push them to the wiki of your own `easybuild-wiki` repository.

#### Setup (only needed once)

```bash
git remote add my_wiki git@github.com:YOUR_GITHUB_LOGIN/easybuild-wiki.wiki.git
git pull my_wiki master
```

#### Update your preview

```bash
git push my_wiki master
```

You can then view your version of the wiki at https://github.com/YOUR_GITHUB_LOGIN/easybuild-wiki/wiki (note: change the `YOUR_GITHUB_LOGIN` part to obtain a valid URL).

### Update

To update your `easybuild-wiki` repository, use:

```
git pull upstream master
```

### Include merged pull requests

**Note: the procedure below requires admin rights on the `easybuild` repository where the EasyBuild wiki is hosted, and thus might not work for you.**

When EasyBuild wiki contribution pull requests are merged in, the following steps should be performed to push the changes through to the actual wiki pages.


#### Setup (only needed once)

```bash
git remote add easybuild_wiki git@github.com:hpcugent/easybuild.wiki.git
```

#### Get merged PRs included in main EasyBuild wiki

```bash
git checkout master
# sync with upstream repo (pull in merged PRs)
git pull upstream master
# pull in wiki edits
git pull easybuild_wiki master
# resolve conflicts, if any
# push latest wiki version to live EasyBuild wiki
git push easybuild_wiki master
```

### Update easybuild-wiki repository with wiki edits

**Note: the procedure below requires admin rights on the `easybuild` repository where the EasyBuild wiki is hosted, and thus might not work for you.**

To update the `easybuild-wiki` repository with updates made directly on the EasyBuild wiki, use:

#### Setup (only needed once)

```bash
# create separate branch for updates made on wiki
git checkout master
git checkout -b easybuild_wiki
```

#### Update

```bash
# check out easybuild_wiki branch (don't make any local commits in this yourself!)
git checkout easybuild_wiki
# pull in wiki edits
git pull easybuild_wiki master
# make sure this branch is in sync with upstream easybuild-wiki repo
git pull upstream master
# push wiki edits to upstream repo
git push upstream easybuild_wiki:master
```
