# git_commits

We are going to need a fresh repository for this workshop.

## Review

Who remembers how to setup a new repo?

1. Open up a terminal window (`CMD` + `SPACE`, type "terminal", hit `ENTER`)
2. Navigate to your desktop, `cd Desktop` (or wherever you prefer)
3. Create a directory: `mkdir git_commits`
4. Navigate inside: `cd git_commits`
5. Initialize git: `git init`

Now we have a new git repository called `git_commits`.

## What are the features of git?

I know most of y'all are probably very familiar already with some of the most common git commands, as we use them every day.

But let's pretend we're not. Let's pretend that all we know is, in this terminal we can type `git`, add a space, and then something comes after that. Also, the internet is out so we can't look it up online.

What do we do then? When it comes to CLI programs, `help` is a great place to start.

There is also a top-level command called `man` that I want to share with y'all, short for "manual", which you can use to pull up detailed information about git, as well as many other shell programs.

(For instance, you can use `man` on many of the native CLI commands we've used before: `ls`, `cd`, `rm`, `mkdir`, `rmdir`).

To use it on git, just type: `man git`.

(This opens up a manual page within your terminal window. You can use your "up" and "down" arrow keys or `j` and `k` to move up and down line-by-line, `SPACE` will jump down the page. To exit, just type `q` for "quit")

If we type `git help`, the CLI prints a usage schematic and a list of concise descriptions on the most common git commands.

It also informs us that we can use `git help -a` to print a list of available subcommands, and `git help -g` to print how to access some concept guides.

And it informs us that if we want even more detailed information about a command, you can type `git help [command]`.

(`git help git` is an alias of `man git`. Just as `git help [command]` is an alias of `man git-[command]`)

So now we have a great deal of information about git and git commands. There are, of course, many resources online as well.

(https://git-scm.com/)

Let's get back to our workshop though.

## How to stage a commit

We have a fresh repository, empty of content, with no commits, and only one branch: `master` (or `main` depending on your git version).

Let's add some commits. But wait, if we try to `git commit` right now we'll get some strange error. Why is that?

Because our directory is empty and we haven't made any changes. So let's create a file.

You can do this either via a text editor, or you can use this handy command:

```sh
echo "Some content" > ./README.md
```

(This creates a file called `README.md` in the current working directory, with the content "Some content")

Now we made a change, we have a file. But if we type `git commit` we will still get an error.

Why is that? The file is saved on our computer, why is it not "saved" in our VCS?

Why do we need to stage our changes before we commit them?

The answer to this has to do with process, and will more sense when we get to branching, but in short:

It gives us developers the ability to explicitly define what a change is, making the process of development incremental.

I also think it has to do with keeping git lightweight.

If you type `git status`, you'll see our `README.md` is an "untracked file". Just because you've initialized a git repository in this folder, it does not mean the contents of this folder are tracked by git.

Now, let's go ahead and stage our `README.md` file by typing `git add .`.

(We are using the `.` here, instead of the file name, to indicate all the contents of the current working directory. To specify the file directly, we'd type `git add README.md`)

Now if we type `git status`, we'll see the "changes to be committed". But lets say it's at this moment that I realize I forgot to add something to our `README.md`. Let's say, instead of "Some content" I want it to read "Hello world".

I could unstage my changes by using `git rm --cached .`, then just make the change and go from there, and let me go through that process just to show it...but we don't actually need to do that.

If we are at that point again where we have a "staged" change, but we want to add something, let's just add the something and see what happens.

```sh
echo "Hello world" > ./README.md
```

Now if I look at the file the contents read "Hello world". But if I do a `git status`, I see my "changes to be committed" and I see "changes not staged for commit".

If I commit right now, I would be committing the file contents "Some content" into the VCS. It's decision time. I have a choice as to what I define and offer up as a "change". How granular do we want to be?

In this case, I don't want to save "Some content", I just want "Hello world", so I type `git add .`, and then we can see with a `git status` that we have only one change staged: Creating a new file called `README.md` with the content "Hello world".

## Committing and showing

Now if we just run `git commit` we will still get this strange error: `fatal: could not read '/some/path/.gitmessage.txt': No such file or directory`

What is that about? Well git allows you to create a commit message template, but we don't have one defined, and I've never really used one.

Instead we can just add a message directly using the `-m` flag, like so: 

```sh
git commit -m "initial commit, new README.md file"
```

There we go! We finally have a commit in this repository. We did it, y'all!

But wait...what does this mean?

Well, it means that, on the `master` branch, we have added a commit. We can see it now if we type `git log`.

There it is. And look, it has our commit message, but it also has the author and a timestamp, plus the word "commit" next to a string of gibberish at the top.

What is that about?

As y'all probably know, every commit includes a unique commit hash, which is a cryptographic checksum. This allows us to identify, with precision, that increment of change.

Now take your cursor, highlight that commit hash, can hit `CMD` + `C` to copy. Now type into your terminal `git show` and then paste that commit hash in, so it looks something like this:

```sh
git show 05462bdede094753ae77fb0fd5c87576cf9b32bf
```

If you hit enter, you'll again see the commit hash, the author, the timestamp, and the commit message, but you'll also see below a diff showing the change.

Let's try that again, but this time only give it the first four characters of that commit hash:

```sh
git show 0546
```










