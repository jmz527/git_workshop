# git_cycle

Repository for demonstrating usage of git

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

### Push

Now, how do we take the changes we made locally and put them up on our source repo?

`git push`, right?

Now, if you just use `git push`, git will assume you are talking about your current working branch. There is a `git push --all` which will push all of your branches (not recommended).

However, if this is a new branch which we created locally, our remote doesn't have a version of it. This is why we use: `git push -u origin [branch_name]`

NOTE: the `-u` here flags to git that we want the branch we are creating on origin to be upstream from our local branch. This way, the next time we run a `git pull`, git will automatically know which branch on origin we're talking about.

Example 1:

Now let's say you're working on a feature branch, you've already pushed it up to origin once or twice before, you've just made a few revisions to your code, you push your branch up and receive a message rejecting your push. What should you do?

I've seen this occur when either, I've reworked the commit history locally, or when commits from someone else have been added to the remote branch.

If you don't care what's on the the origin branch, all you care about is what you have locally, you can run `git push -f origin [branch_name]` to "force" the push.

This will replace what's up on origin with your local commit history. Not recommended if there was someone reviewing your PR.

Example 2:

If you do care about what's on the origin branch, then I would do the following:

```sh
git log
git branch -m [branch_name]_DANGLE
git checkout [branch_name]
git reset --hard [previous_commit]
git pull
git cherry-pick [commit]^..[commit]
git push
```

