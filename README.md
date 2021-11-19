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
