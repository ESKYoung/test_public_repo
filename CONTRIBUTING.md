# Git strategy for developing public repositories

We need to develop new code in feature branches that may be subject to history rewriting **without** impacting users
who fork our public repositories. History rewriting actions include rebasing, squashing, and fixing up Git commits.

After assessing different Git strategies, we have decided to develop features on a private mirror of the public Git
repository. This mirror will allow us to develop features without impacting forks of the public repository, including:

1. Rebasing features with the latest updates of the public `master`;
2. Squashing/fixing up commits in the feature branch, as developments are made;
3. Ensuring business continuity by backing up features to the remote; and
4. Enabling collaboration between team members<sup>*</sup>.

Although developments should be made in the private mirror, **Pull Requests (PRs) should be raised on the public
repository for transparency**.

<sup>*</sup>On the understanding that feature branches are subject to history-rewriting.

## Acknowledgments

This Git strategy has been devised from the following [StackOverflow answer](https://stackoverflow.com/a/30352360).

## Definitions

We will use the following definitions:

* `public`: The public Git repository remote
* `origin`: The private mirror remote

## General rules

1. **Never make history rewriting changes to any commits in `public`**;
2. **Never make history rewriting changes to any commits in `public`**;
3. **Never force push on `public`**;
4. Keep your `origin` feature branches up-to-date by rebasing off the latest `master`
   * Be responsible and help others - regularly 
   [pull `public` `master`, and push to `origin` `master`](#updating-master-on-the-private-mirror) to get the latest 
   `master`
5. Only develop on your own `origin` feature branch;
   * **Do not** develop a feature branch that is not yours without explicit permission from the owner.
6. Feature branches should come off `origin` `master`, and never be created in `public`;
7. [Raise PRs on `public` only](#raising-pull-requests-on-the-public-git-repository), and **do not make history
rewriting changes to these commits**
   * Try to make any further commits in `origin` only.

## Instructions

After [setting up a private mirror](#setting-up-a-private-mirror), and
[cloning it locally](#cloning-the-private-mirror), the rest of the instructions assume you are working out of your
local private mirror.

The instructions are mostly command line-based. However, some IDEs, like PyCharm, have UIs for selecting different
Git remotes.

**Once per public Git repository**:

1. [Setting up a private mirror](#setting-up-a-private-mirror)

**Once per developer**:

1. [Cloning the private mirror](#cloning-the-private-mirror)
2. [Set up the public Git repository remote](#set-up-the-public-git-repository-remote)

**Regularly per developer**:

1. [Updating `master` on the private mirror](#updating-master-on-the-private-mirror)
2. [Pushing to the private mirror](#pushing-to-the-private-mirror)
3. [Raising Pull Requests on the public Git repository](#raising-pull-requests-on-the-public-git-repository)

### Setting up a private mirror

Create a new, empty private Git repository for the private mirror on your remote, e.g.
[GitHub](https://help.github.com/en/github/creating-cloning-and-archiving-repositories/creating-a-new-repository).

Create a bare clone of the public Git repository.

```
git clone --bare <<<PUBLIC GIT REPOSITORY URL>>>
```

In your bare clone, push up to the private mirror, and then delete the bare clone.

```
cd <<<BARE CLONE FOLDER NAME>>>
git push --mirror <<<PRIVATE MIRROR URL>>>
cd ..
rm -rf <<<BARE CLONE FOLDER NAME>>>
```

### Cloning the private mirror

If you do not have the private mirror locally, clone it, and `cd` into it.

```
git clone <<<PRIVATE MIRROR URL>>>
cd <<<PRIVATE MIRROR FOLDER NAME>>>
```

You can now make and commit your developments in feature branches.

### Set up the public Git repository remote

To pull changes and push feature branches for PR to `public`, set up a Git remote.

```
git remote add public <<<PUBLIC GIT REPOSITORY URL>>>
```

### Updating `master` on the private mirror

To update `origin` `master` with the latest changes in `public`, first checkout `master` locally.

```
git checkout master
```

Pull any new changes from `public` `master`, and then push them to `origin` to update `origin` `master` with the
latest changes.

```
git pull public master
git push origin master
```

You can now rebase your feature branches onto the latest `master`.

### Pushing to the private mirror

Only create feature branches in `origin`, **not** `public`. This can be done on the remote, or locally; if locally, the 
first time you push to the remote, you will need to specific the `--set-upstream` option.

```
git push --set-upstream origin <<<FEATURE BRANCH NAME>>>
```

### Raising Pull Requests on the public Git repository

Once you are happy for your feature branch to be reviewed, push to the `public` remote. **If you do this, none of these 
commits should be changed**, as they are now public - **never force push**.

```
git push public <<<FEATURE BRANCH NAME>>>
```

Now you can raise your PR as normal.
