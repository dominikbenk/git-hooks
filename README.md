# Our notes
### Main sources
https://github.com/aitemr/awesome-git-hooks

https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet

[nice tutorial](https://www.atlassian.com/git/tutorials/git-hooks)

## Distributing the work
*feel free to add new subtopics / assign your name to them*


- What are the git hooks and why it is good to use them?      **Kuba**
- Setting up the hooks **Dominik**
- Client-side vs Server-side hooks **Kuba**
- Sharing hooks with others **Dominik**
- Usage (describe the most interesting hooks in samples, there are also other hooks not included, see https://git-scm.com/docs/githooks)
----------------------------------------------------------
# Git Hooks
Group project for Version control with Git (JEM224)

### Authors
Nelli Kalashyan, Jakub Černý, Dominik Benk

## What are Git Hooks?
Git hooks are a handy tool that can boost our efficiency and make our collaboration with other colleagues a bit easier. They are basically executable scripts that are triggered by classical git commands (to which we assign them). They can be found in *.git/hooks* folder in every repository and they can affect the Git commands arbitrarily. The scripts themselves can be shell, Python or anything else, which is very practical since everyone can use his or her favourite programming language.

As mentioned above, the main goal is to make things easier and therefore the hooks can be used e.g. for custom specifications of a team of developers (or possibly a single developer handling a lot of work) to secure proper workflow, format of commit messages and similar things. More information about how to set up the hooks, how to share them with a team of collaborators or what are their specific applications can be found in next sections.

## Types of Hooks
* Client-side Hooks 

	The first group of hooks, as the name suggests, is intended for someone who contributes to a repository and wants to do it in a more efficient way. The are for instance 	 hooks that affect the commiting process- it is possible to inspect content of the snapshot (to run unit tests, to check for mistakes in code, etc.), to check whether the 	   commit message is written according to given requirements or to simply show certain notifications. Furthemore, the client-side hooks can be used e.g. to notify 		collaborators that a commit has been done, to validate accomplished changes before a push or to affect actions of Git's garbage collector

* Server-side Hooks

	Those hooks are created by an administrator in order to enforce some rules for contributors. It is possible to interupt someone's push e.g. when it does not 		satisfy requirements or when it modifies content that is not allowed to change. At the same time, additional information about pushed content can be collected or 		notifications can be sent after a successfull push.

## Sharing Git Hooks with a Team
Since Git hooks are by default part of the .git folder, they are ignored and can't be added to staging index.

One of the solutions is to create a new hook folder in the root path of repository, so the hooks can be version controlled.
To let Git know, where you want him to look for the hook scripts, you can simply set the default hook path via
```sh
git config core.hooksPath custom-hooks
```
## Setting up Git Hooks
### Sample Hooks
Whenever you initialize a Git repository, you are provided with some default set of templates. These are copied from
```C:/Program Files (x86)/Git/share/git-core/templates/``` to ```.git/hooks``` and are disabled from beginning. To make them executable, one simply need to remove *.sample* from their file names. They also include detailed description of what they do or when they trigger, but more on this [later](#application).

You're of course free to edit them to fit your needs, but keep in mind, that they are usually written in SHELL.

### Custom Hooks
As already mentioned, your hooks can trigger any executable script. To demonstrate this, we will go step by step setting up a simple hook with python script.

After following the [setup of a custom Git hook folder](#sharing-git-hooks-with-a-team), we can create the script we want to be triggered. The name of the file is very important, as it determines what event / Git command triggers it. For purpose of this tutorial, we chose pre-push, which is usually used to prevent *git push* from happening under certain conditions. 

Also note that, to replicate this, you need to have python installed along with *playsound* package ```pip install playsound```

The script itself is very simple. The first line is so called **Shebang**, which is there to tell in which language interpreter should it be executed.
Condition ```__name__ == "__main__"``` is in our case redundant, as we will always execute this script as a main program, but could be important for more complicated modules.
Finally, there is a script that plays an mp3 file.
```python
#!/usr/bin/env python
import playsound
if __name__ == '__main__':
	playsound.playsound('custom-hooks\props\push.mp3', True)
```
It is also important to make sure the file is marked as executable, this can be done by running the following command
```sh
chmod +x pre-push
```
And now, you're ready to push!


## Application
- applypatch-msg

This hook is called with the git-am command. It takes a single parameter - the name of the file in which the log message of the proposed commit is stored. 

- pre-applypatch

This hook is called with the git-am command. It takes no parameters and is called after applying the patch, but before committing.

- post-applypatch

This hook is called with the git-am command. It does not take any parameters and is called after the patch and commit have been applied.
This hook is mainly for notification and cannot affect the result of *git am*.

- pre-commit

This is performed before a commit is created.

1. The user writes *git commit -v* in the terminal;
2. git tries to do a pre-commit hook locally on the developer's machine;
3. If the hook fails, it will abort the commit operation;
4. If the hook completes without errors, the commit operation continues and a text editor opens to enter the message.

- pre-merge-commit

This hook is called by git-merge and can be accessed using the *--no-verify* option. It takes no parameters and is called after a successful merge and before the proposed commit log message to execute the commit. Exiting this script with a non-zero status causes the *git merge* command to abort before the commit is created.
The pre-merge-commit hook by default, if enabled, triggers the pre-commit hook if the latter is enabled.

This hook is invoked with the *GIT_EDITOR=*: environment variable if the command will not invoke the editor to change the commit message.


- prepare-commit-msg

This hook is called by *git-commit* immediately after the default log message is prepared and before the editor is started.

- commit-msg

The commit-msg hook takes one parameter - the path to the temporary file containing the commit message, specified by the developer.

- post-commit

The *post-commit* hook runs after a commit has been created. It takes no parameters, but you can easily get information about the last commit by running git log -1 HEAD. Typically, this script is used for notifications or something similar.

- pre-rebase

The *pre-rebase* hook runs when you try to rebase and can stop the process by returning a non-zero code. It can be used to disallow rebasing commits that have already been sent. 

- post-checkout

This hook can be used to customize the working directory according to the requirements of the project. For example, moving large binary files that should not be tracked to the working directory, autogenerating documentation, etc.

- post-merge

This hook can be used to retrieve data in your working directory that Git can't track, such as permissions. This hook can also check for files external to Git that you might want to copy when you make changes.

- pre-push

This hook is used after updating the deleted links, but before sending the data directly. It takes the name and path of the remote repository as parameters, and the list of changes to send via *stdin*. It can be used to validate a set of changes before actually sending them.

- pre-receive

This hook is started first when you start receiving data from the client. It gets a list of changes sent to *stdin* and if it ends with a non-zero code, none of them will be accepted. This hook can be used to make sure that all changes can be applied by fast-forwarding, and also to check access rights.

- update

The update hook is very similar to *pre-receive*, except that it is executed for each branch that the sender tries to update.

- proc-receive

This hook is called by git-receive-pack. If the server has set the *receive.procReceiveRefs* multi-valued configuration variable, and the commands sent to receive-pack have matching reference names, those commands will be executed by this hook, not by the internal *execute_commands()* function.

- post-receive

This hook is called by git-receive-pack when it responds to *git push* and updates the links in its repository. It is executed once on the remote repository after all the links have been updated. It is superior to the post-update hook in that it gets the old and new values of all references in addition to their names.

- post-update

This hook is called by git-receive-pack when it responds to *git push* and updates the links in its repository. It is executed on the remote repository once after all the links have been updated.
It takes a variable number of parameters, each of which is the name of a link that has been updated.

This hook is primarily for notification and cannot affect the result of *git receive-pack*.

- reference-transaction

This hook is called by any Git command that updates references. It runs every time a reference transaction is prepared, committed, or cancelled, so it can be called several times.

- push-to-checkout

This hook is called by git-receive-pack when it responds to *git push* and updates the links in its repository, and when push tries to update a branch that is currently checked and the *receive.denyCurrentBranch* configuration variable is set to *updateInstead*.

- pre-auto-gc
- post-rewrite
- sendemail-validate
- fsmonitor-watchman
- p4-changelist
- p4-prepare-changelist
- p4-post-changelist
- p4-pre-submit
- post-index-change


