# Our notes
### Main sources
https://github.com/aitemr/awesome-git-hooks

https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet

[nice tutorial](https://www.atlassian.com/git/tutorials/git-hooks)

## Distributing the work
*feel free to add new subtopics / assign your name to them*


- What are the git hooks and why it is good to use them?      **Kuba**
- Setting up the hooks **Dominik**
- Client-side vs Server-side hooks
- Sharing hooks with others **Dominik**
- Usage (describe the most interesting hooks in samples, there are also other hooks not included, see https://git-scm.com/docs/githooks)
----------------------------------------------------------
# Git Hooks
Group project for Version control with Git (JEM224)

### Authors
Nelli Kalashyan, Jakub Černý, Dominik Benk

## What are git hooks and why is it good to use them?
Git hooks are a handy tool that can boost our efficiency or make our collaboration with other colleagues a bit easier. They are basically executable scripts (those can be shell, Python or other scripts) located in *.git/hooks* folder that are triggered by classical git commands and executed in order to affect them the way we want. 


## Setting up the Hooks
### Sample Hooks
Whenever you initialize a Git repository, you are provided with some default set of templates. These are copied from
```C:/Program Files (x86)/Git/share/git-core/templates/``` to ```.git/hooks```

and are disabled from beginning. To make them executable, one simply need to remove *.sample* from their file names. They also include detailed description of what they do or when they trigger, but more on this [later](#application).

You're of course free to edit them to fit your needs, but keep in mind, that they are usually written in SHELL.

### Custom Hooks
As already mentioned, your hooks can trigger any executable script. To demonstrate this, we will go step by step setting up a simple hook with python script.

First of all, we opted to create a custom Git hook folder, mainly for [sharing reasons](#sharing-hooks-with-others).
To let know Git, where you want him to look for the hook scripts, you can simply set the default hook path
```sh
git config core.hookspath custom-hooks
```


Now, you're ready to create the script to be triggered. The name of the file is very important, as it determines what event / Git command triggers it. For purpose of this tutorial, we chose pre-push, which is usually used to prevent *git push* from happening under certain conditions. Also note that, to replicate this, you need to have python installed along with *playsound* package:
```sh
pip install playsound
```
The script itself is very simple. The first line is so called **Shebang**, which is there to tell in which language interpreter should it be executed.
Condition ```__name__ == "__main__"``` is in our case redundant, as we will always execute this script as a main program, but could be important for more complicated modules.
Finally, there is a script that plays an mp3 file.
```python
#!/usr/bin/env python
import playsound
if __name__ == '__main__':
	playsound.playsound('custom-hooks\props\push.mp3', True)
```
Also note that, to make sure the file is marked as executable, we need to run
```sh
chmod +x pre-push
```
And now, you're ready to push!

## Sharing hooks with others

## Application
- applypatch-msg
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


