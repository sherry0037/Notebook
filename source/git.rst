================================
Git Commends Reference
================================
Some useful git commends.

----------------------------------
How to undo commits/add?
----------------------------------
- Undo the latest commit and keep changed files:
    `git reset --soft HEAD~1`

- Undo the latest commit and discard all changes:
    `git reset --hard HEAD~1`

- Undo adding a file:
    `git reset [filename]`

- Undo adding all files:
    `git reset`

----------------------------------
How to modify commits?
----------------------------------
- Modify most recent commit:
    `git commit --amend`

- Modify older commits:
    For example, if you want to modify commit bbc643cd, run

    `git rebase --interactive 'bbc643cd^'`

    to rebase back to the commit before the one you wish to modify.

    In the default editor, modify pick to edit in the line mentioning 'bbc643cd'.

    Do any change normally. After editing, type

    `git rebase --continue`

    to return back to HEAD.