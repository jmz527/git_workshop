# git_branches

Repository for demonstrating usage of git

## Follow-up from last session

List all commits?

`git log --reflog`
`git rev-list --remotes`
`git rev-list --all --remotes`


Let's say we made several changes in our repo, we haven't committed any of these changes, but we don't like any of it and just want to start over. What can we do?

`git checkout -- .`

Let's say we made several changes that we realize should be included in the previous commit. What can we do?

```sh
git add .
git commit --amend --no-edit
```

Let's say we want to change the message of the last commit:

```sh
git commit --amend -m "new message"
```

You can make empty commits:

`git commit --allow-empty -m ""`


## branches

Branches are just pointers.

`git branch` will show "local" branches.
`git branch --remotes` will show "remote" branches.

### How do we create a branch?

```sh
git branch [branch_name]
git checkout [branch_name]
```

```sh
git checkout -b [branch_name]
```

NOTE: This creates the branch from the HEAD of your current branch.

SIDE NOTE: I have an alias in my bash profile for this.


### How do we delete a branch?

```sh
git branch -D [branch_name]
```

### How do we rename a branch?

```sh
git branch -m [branch_name] [new_branch_name]
# or
git branch -m [new_branch_name]
```

### Detached HEAD state

We switch between branches using `git checkout [branch_name]`. We can also switch between commits using `git checkout [commit]`. This puts us in a 'detached HEAD' state. No longer in the head of the branch.

```sh
git log --graph --full-history --all --color --pretty=format:"%x1b[31m%h%x09%x1b[32m%d%x1b[0m%x20%s"
```

### Stashing

`git stash`
`git stash apply`

### Branching strategy

Example 1:

When developing out a new feature, let's say we're a handful of commits into a branch when we realize we want to take the whole thing in a new direction.

This is what I do in that scenario:

```sh
git log
git branch -m [branch_name]_DANGLE
git checkout [commit]
git checkout -b [branch_name]
```

Now we can develop down a new branch, and I've saved my old branch in case I missed something.

`git checkout -b [branch_name]_EXPLORER`
`git checkout -b [branch_name]_HOTFIX`

Example 2:

Let's say, we're in the same scenario, handful of commits in, and we want to take the whole thing in a new direction, but we want to just throw away the work we've done?

```sh
git log
git reset --hard [commit]
```

NOTE: Those commits still exist, they are just not part of the commit history.

If you run `reset --soft`, the commits you are trying to throw away will show up in the index (a.k.a. the stage).


### Fetch vs Pull

Concept of upstream and downstream

Remember, your `integration` branch is downstream from our remote `integration` branch.

How do we get the changes from our remote `integration` branch?

`git fetch`

So what is `git pull`?

`git fetch` + `git merge`.

This means, `git fetch` syncs your local `origin/integration` with the remote `integration` branch, then runs a `git merge` between local `origin/integration` and local `integration`. Every time you pull, you are triggering a merge.

Example:

Let's say you turn on your computer one day, checkout a branch, and run `git pull` which results in a merge conflict. What do you do?

This most likely happens when...

```
git checkout -b [branch_name]_TEMP
git branch -D [branch_name]
git checkout [branch_name]
```

# git_merging

### Merging + commit history

Let's say we want to merge a feature branch into another branch, let's say `integration` (local).

We want to make sure we are on `integration`.

```sh
git checkout integration
git merge [feature_branch]
```

This will open up an editor. You can add a message here if you want, but usually we'll just `CTRL` + `X` to get out of this.

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

When merge conflicts arise it means, between the two branches, both have at different points modified the same lines within a file.

Let's create a conflict.

We're still on the `integration` branch. The file contains both changes, but also contain changes added by git.

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


# git_rebasing

### Rebasing vs Rebase --onto

Re-apply commits on top of another base tip

```sh
git rebase master
git rebase master [feature_branch]
```

Does this create new commits? Yes.

Transplant a [feature_branch] based on one branch to another:

```sh
git rebase --onto master [branchA] [branchB]
```

Can also:

```sh
git rebase --onto master [commit] [branchB]
```

Useful commands:

```sh
git rebase --continue
git rebase --abort
```

### Interactive Rebasing

```sh
git rebase -i [commit]
git rebase -i [commit] [feature_branch]
```

This will open up an editor which you can use for various things:

- Re-arrange commits
- Drop commits
- Reword the commit message
- Squash commits together
- Go back and edit a commit

