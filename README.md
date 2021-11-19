# git_merges

Repository for demonstrating usage of git

# git_merging

### Merging + commit history

"Join two or more development histories together"

Let's say we want to merge a feature branch into another branch, let's say `integration` (local).

We want to make sure we are on `integration`.

```sh
git checkout integration
git merge [feature_branch]
```

This will open up an editor, nano or vim. You can add a message here if you want, but usually we'll just `CTRL` + `X` to get out of this.

This will replay the changes made on the [feature_branch] branch since it diverged from integration until its current commit (C) on top of integration, and record the result in a new commit (contains the names of the two branches).

Notice how the commit history has been rewritten. Any commits made in integration that have a timestamp coming after the commits within [feature_branch], those commits are still ahead of the commits from [feature_branch].

### Squash Merging

We have a policy of squash merging our commits.

To do this locally:

```sh
git checkout integration
git merge --squash [feature_branch]
```

This does not create a commit like `git merge`, instead this adds all the changes from [feature_branch] into the index (stage) of the integration branch.

Notice how the commit history has NOT been rewritten. All the changes are captured in a single new "squash merge" commit.

### Merge conflicts

When merge conflicts arise it means, between the two branches, both have at different points in their development histories modified the same lines within a file.

Let's create a conflict.

We're still on the `integration` branch. The file contains both changes, but also contains changes added by git.

```js
<<<<<<< HEAD
I worry about conflicts
=======
blah
>>>>>>> integration_FEATURE_BRANCH
```

In the "HEAD" is the change from your current branch. Below is the change from the feature branch.

`git merge --abort`

Or, make changes and when done:

```sh
git add .
git merge --continue
```

This will pull up an editor again. Notice, the suggested commit message. `CTRL` + `X` to get out.

NOTE: The resolve occurs in the merge commit.
