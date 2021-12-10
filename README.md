# git_workshop

Repository for demonstrating usage of git

# Cherry-Picking

Let's say you have two divergent branches, one older and one newer. Let's call them `old_branch` and `new_branch`. You're working on the newer one when you realize there is one change that you made in the older branch that you can apply to the new one.

If that change is cleanly contained within a commit, then you can find that commit hash and apply it directly like so:

```sh
git log old_branch
# copy commit hash
git cherry-pick [commit hash]
```

# Resolving Cherry-Picking Conflicts

Now this will work as long as...

1. the change is cleanly contained within a commit, and
2. none of the changes within your current branch conflict with the change you are cherry-picking

Failing the second of these will result in a merge conflict.

If you were not expecting a merge conflict and would like to cancel the cherry-picking operation, you can enter `git cherry-pick --abort`. This will cancel the operation and return to the pre-sequence state.

If you decide to resolve the merge conflict, here are the steps you would follow:

```sh
git cherry-pick [commit hash]
# results in merge conflict
# modify the files to resolve the conflict
git add .
git cherry-pick --continue
```

This will open up an editor given you the opportunity to edit the commit message.

# Bulk Cherry-Picking

Let's say were back in the same situation again: you have two divergent branches, one older and one newer.

This time there are several changes that you made in the older branch that you want to apply to the new one, and luckily, they are all sequential.

You can apply them all in one go, like this:

```sh
git log old_branch
# copy first and last commit hashes
git cherry-pick [first hash]^..[last hash]
```

This will apply those changes onto your current branch, one at a time.

If one of these causes a conflict, the operation will stop mid way and give you the opportunity to resolve it. It's here where you can again use `cherry-pick --abort`.

Additionally you can use `cherry-pick --skip` if you want to ignore this one but still apply the rest of the sequence.

And, of course, you can also resolve the conflict, `git add .` and `git cherry-pick --continue`.
