# git_basics

Now some of you may use an IDE that is integrated with git, and that's totally fine for your day-to-day, but for the purpose of these workshops we are going to use git directly within your terminal's CLI.

## How does one setup a git respository?

There is a way to create a git repository on github, but we are going to ignore that for now. Instead, we are going to create a folder on our desktop and turn it into a repository.

Now first, open up a terminal window (`CMD` + `SPACE`, type "terminal", hit `ENTER`).

Navigate to your desktop, `cd Desktop` (or to wherever you prefer really).

You all should have git installed, but type `which git`  just to make sure.

(This will display the directory path to your "git" binary. It should look something like this: `/usr/local/bin/git`).

Now let's create a directory and then navigate inside:

```sh
mkdir git_basics
cd git_basics
```

(This `mkdir` is the command to make a directory, in this case named `git_basics`, and the `cd` is the command to "change directory")

This is just an empty folder on your desktop (you can see it in your GUI), it is not a repository yet, and we can see this by trying to use a git command.

Try `git status` and you should see: `fatal: not a git repository (or any of the parent directories): .git`

To make it a git repository, type `git init`.

Now it's a git repository, and you are either on the `master` or `main` branch.

Try a `git status` now, and you should see the following:

```sh
On branch master

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

## What makes it a git repository?

For those who are curious, run this command:

```sh
ls -a
```

(This is just the command to list the contents of your present working directory, with a `-a` flag so that it includes files and folders that begin with a `.`)

(Files or folders that begin with a `.` are called dotfiles, and are an easy way of hiding content that you'd rather not see in the GUI)

You'll notice a directory has been added to your folder named `.git`. This folder contains all the information required for version control of the repostory.

Let's see what happens if we remove this `.git`

Type the following: `rm -rf .git`

(This `rm` is the command to remove a file. This normally wouldn't work with a folder, but we added the `-f` force and the `-r` recursive flags)

Now if you run `ls -a`, you should no longer see `.git`.

Your `git_basics` directory is no longer a git repository. We've turned it back into an ordinary folder.

To remove the folder, just type:

```sh
cd ..
rmdir git_basics
```

(Here is that `cd` "change directory" command again, this time with a `..` which takes you up to the parent directory. Then the `rmdir` command is for removing a directory)
