# Notes for Git

## Local Working flow

This flow will use git as a local version manager to guard
against fat fingers. Also it will help to revert to a previous
version easily.

Assume you are under the folder `~/code`.

> `git status`

This will tell you whether it's under a git folder or not.
If it says `fatal: Not a git repository (or any of the parent directories): .git`
Then it means it is not a git folder

> `git init`

This command creates a new git repository under the current
folder. It will add a new hidden folder `.git` under the current
folder. The output will look like: `Initialized empty Git repository in ~/code/.git/`
Now you can add some files to this folder like a normal folder. If there is
already folder in this folder, you are good to go.

> `git status`

This will show the status of the changes you have made to the current folder.
You may see some files under the `Untracked files:` section. Those are not tracked
by git.

> `git add .` or `git add <filename>`

This add all untracked files in current folder or the specfic file to the git.
So when you type `git status` again, they will not show under `untracked files`
anymore.

> `git commit -am "<Your commit message>"`

This will commit all the current changes to the local git repository.

> `git log`

This will show you commit history. Each commit has a hash tag, like 5073e20a21f0d03bf03e81247fffa31d8a7bc004.

> `git show <sha>`

This will show the changes of the commit `<sha>`. This is helpful to tell you
the changes you made previously.

> `git show <sha>:<filename>`

This will show the content of the `<filename>` in the `<sha>`. This is useful
when something is broken on HEAD. And you want to see what is in the file in an
older version.
