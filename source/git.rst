================================
Git Commands Reference
================================
Some useful git commands.


----------------------------------
Create new branch
----------------------------------
``git checkout -b [name_of_your_new_branch]``


----------------------------------
How to undo commits/add?
----------------------------------
- Undo the latest commit and keep changed files:
    ``git reset --soft HEAD~1``

- Undo the latest commit and discard all changes:
    ``git reset --hard HEAD~1``

- Undo adding a file:
    ``git reset [filename]``

- Undo adding all files:
    ``git reset``


----------------------------------
How to modify commits?
----------------------------------
- Modify most recent commit:
    ``git commit --amend``

- Modify older commits:
    For example, if you want to modify commit bbc643cd, run

    ``git rebase --interactive 'bbc643cd^'``

    to rebase back to the commit before the one you wish to modify.

    In the default editor, modify pick to edit in the line mentioning 'bbc643cd'.

    Do any change normally. After editing, type

    ``git rebase --continue``

     to return back to HEAD.

-----------------------------------
How to get new changes from Master
-----------------------------------

Two options: ``merge`` and ``rebase``.

- You can merge the ``master`` branch into the ``feature`` branch using:

  ``git merge feature master``

  This will create a new merge commit.

- Or you can rebase the ``feature`` branch onto ``master`` branch using:

  ``git checkout feature``

  ``git rebase master``

  This moves the entire ``feature`` branch on the tip of the ``master`` branch, without leaving a commit message.

----------------------------------
Different ways to do ``git pull``
----------------------------------
- ``git pull`` does a ``git fetch`` to pull down data from the specified remote, and then calls ``git merge`` to join the changes received with your current branch’s work.

- Use ``git pull --rebase`` to reapply your work on top of the incoming changes.
    ``git pull --rebase <remote name> <branch name>``


----------------------------------
Stash the changes
----------------------------------
- Use ``git stash`` to save all the changes.

- Use ``git stash apply`` to get the latest stashed changes back.


----------------------------------
See changes in a commit
----------------------------------
- Use ``git diff-tree --no-commit-id --name-only -r bd61ad98`` to see all the files added in a commit

- Use ``git diff COMMIT~ COMMIT`` to see the changes in a commit


----------------------------------
Push specific commit
----------------------------------
- ``git push origin 1d9ec65c09b07661b7daff31c810d365d1c73be4:refs/heads/master``


----------------------------------
Delete git repository
----------------------------------
- Make a repository no longer a git repository: ``rm -rf .git``
