# git_rebasing

Repository for demonstrating usage of git

# git_rebasing

### Rebasing

Re-apply commits on top of the latest commits from the base branch

```sh
git rebase master
git rebase master [feature_branch]
```

`man git-rebase` has a great visual for these.

Does this create new commits? Yes.

Same timestamps on the commit, but new commit hashes. This means we can have a commit history with timestamps in non-chronological order.

### Rebasing onto

Transplant a [feature_branch] based on one branch onto another:

```sh
git rebase --onto master [branchA] [branchB]
```

`branchB` is our current branch. `branchA` is the branch our current branch (`branchB`) is based upon. We want to rebase onto `master`, which means all the commits between the head of `branchA` and `branchB` are applied onto `master`.

All of the commits between `master` and `branchA` still exist within `branchA` but, post-rebase, they will no longer be in `branchB`.

Now we don't always create `fork` branches to mark where two branches development histories diverged. This is the main reason why I have developed the habit of including an empty commit at the beginning of a new branch.

Can also:

```sh
git rebase --onto master [commit] [branchB]
```

### Rebasing conflicts

If you run into a conflict while rebasing, this means that the execution of the rebase haulted part way through.

When this happens to me while rebasing, I pretty much always use `git rebase --abort` to go back and reassess. Comb through the commit history and understand the conflict before trying again.

If you go through with resolving the conflict, you can then:

```sh
git add .
git rebase --continue
```

Just remember, this changes the commit. Where before you may have been adding a new line of code, now you are changing an existing line of code.

The result is the same codebase but the development history is not the same. And this might mean you can no longer expect the same functionality from any of the previous commits, pre resolution commit, within this branch.

### Interactive Rebasing

```sh
git rebase -i [commit]
git rebase -i [commit] [feature_branch]
```

This will open up an editor which you can use for various things:

- Re-arrange commits
- Squash commits together
- Drop commits
- Reword commit messages
- Go back and edit a commit

This, in my opinion, is the ultimate tool in git. This allows you to truly re-write your development history.

Let's try one of these...

#### Re-arranging commits

To do this, all you need to do is re-arrange the order of these commits in this editor window.

In vim, it's easiest to navigate to the line you want using arrow keys or `hjkl`, then use `dd` to cut the line and then `p` to paste it where you want it.

#### Drop commits

For this, all you need to do is add `drop` or a `d` next to the commit in this editor window.

In vim, navigate to the line you want, hit `x` four times to remove `pick`, hit `i` (to "insert"), hit `d` then `ESC` to get out of "insert" mode.

#### Reword commit messages

Same steps as dropping a commit, except instead of `d`, you will want to enter `reword` or `r`.

This should hault the execution of the rebase at that point, allowing you to edit the commit message.

#### Squash commits together

Also the same steps as dropping a commit, except this time you want to enter `squash` or `s`.

Important note: This uses the commit but melds into previous commit in the series. And, once you execute the rebase, it will hault the execution at that commit to amend the the commit messages together.

#### Go back and edit a commit

Again, same steps as the others, except this time you want to enter `edit` or `e`.

This will hault the execution of the rebase at that commit with that commits changes in staging, so it's as if you just used `git add` on them.

You can now make the edits you want to make. When you're done:

```sh
git add .
git rebase --continue
```
