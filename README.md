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

## What are Git Hooks and why is it good to use them?
Git hooks are a handy tool that can boost our efficiency and make our collaboration with other colleagues a bit easier. They are basically executable scripts that are triggered by classical git commands which they are connected to. They are located in *.git/hooks* folder in every repository and they can affect the Git commands arbitrarily. The scripts themselves can be shell, Python or anything else, which is very practical since everyone can use his or her favourite programming language.

As mentioned above, the main goal is to make things easier and therefore the hooks can be used e.g. for custom specifications of a team of developers (or possibly a single developer handling a lot of work) to secure proper workflow, format of commit messages and similar things. More information about how to set up the hooks, how to share them with a team of collaborators or what are their specific applications can be found in next sections.

## Client-side vs Server-side Hooks
The first group of hooks, as the name suggests, is intended for someone who contributes to a repository and wants to do it in a more efficient way. The are for instance hooks that affect the commiting process- it is possible to inspect content of the snapshot (to run unit tests, to check for mistakes in code, etc.), to check whether the commit message is written according to given requirements or to simply show certain notifications. Furthemore, the client-side hooks can be used e.g. to notify collaborators that a commit has been done, to validate accomplished changes before a push or to affect actions of Git's garbage collector

The server-side hooks are created by an administrator in order to enforce some rules for contributors. It is possible to interupt someone's push e.g. when it does not satisfy requirements or when it modifies content that is not allowed to change. At the same time, additional information about pushed content can be collected or notifications can be sent after a successfull push.

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

This hook is called with the git-am[1] command. It takes a single parameter - the name of the file in which the log message of the proposed commit is stored. Exiting with a non-zero status causes git-am to terminate before applying the fix.
Hook is enabled to edit the message file in place and can be used to normalize the message into some standard project format. It can as well be used to uncommit after the message file has been checked.
By default, the applypatch-msg hook, if enabled, starts the commit-msg hook if the latter is enabled.

- pre-applypatch
- post-applypatch
- pre-commit
- pre-merge-commit
- prepare-commit-msg
- commit-msg
- post-commit
- pre-rebase
- post-checkout
- post-merge
- pre-push
- pre-receive
- update
- proc-receive
- post-receive
- post-update
- reference-transaction
- push-to-checkout
- pre-auto-gc
- post-rewrite
- sendemail-validate
- fsmonitor-watchman
- p4-changelist
- p4-prepare-changelist
- p4-post-changelist
- p4-pre-submit
- post-index-change


