# Git Branching Model

The workflow below assumes your repo contains existing `master` and `development` branches. This branching model is based on Vincent Driessen's [A successful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/), which presents an excellent visual and programmatic guide to git branching with style.

### Feature branches

Feature branches are used to implement new enhancements for upcoming releases. A feature branch should be *ephemeral*, i.e. it should only last as long as the feature itself is in development. Once the feature is completed, it must be merged back into the `development` branch and/or discarded.

Consider an example in which we to implement a new feature called `foo`:

Begin by switching to a new branch `foo`, branching off of `development`:
```
$ git checkout -b foo development
```
You should use `foo` to implement and commit all changes required for your new feature. When your feature is complete, merge your feature branch back into `development`:
```
$ git checkout development
$ git merge --no-ff foo
```
Then, push your local changes to the remote repository:
```
$ git push origin development
```
Finally, delete your feature branch:
```
$ git branch -d foo
```

### Hotfix branches

Hotfix branches are used to implement critical bug fixes in the *production version* of your code, i.e. code that is currently being used for experimental analysis. As such, they should always originate from the `master` branch, who's version tag must be incremented after the hotfix branch is merged.

First, identify the current tag for the master:
```
$ git describe --tags
```
Now, consider an example in which we discover a critical bug in version 0.5.3 of the current production release of your code:

Begin by switching to a new branch `hotfix-0.5.4`, branching off of `master`:
```
$ git checkout -b hotfix-0.5.4 master
```
Implement your bug fix in `hotfix-0.5.4`, commit your changes, then merge your hotfix branch back into `master`:
```
$ git checkout master
$ git merge --no-ff hotfix-0.5.4
```
Don't forget to increment the patch value of your version tag by one and push all changes to the remote:
```
$ git tag v0.5.4
$ git push origin master --tag
```
Finally, merge the hotfix into `development` and push to the remote:
```
$ git checkout development
$ git merge --no-ff hotfix-0.5.4
$ git push origin development
```
Then delete your hotfix branch:
```
$ git branch -d hotfix-0.5.4
```