.. _git_usage:

========================================================================
Git General
========================================================================

Git is a development version control tool.

Get Git Access
---------------

Ask your local Git administrator for access to your organization's Git
repositories. Every organization is different.

Setup Your GitHub/GitLab Credentials
------------------------------------------------------------------------

.. important::

   Make sure the one of the first things you do when you setup your development
   system is to setup your github credentials and identifying username and
   email. This is to assist in tracking and documenting development
   history::

      git config --global user.name "Jane Smith"
      git config --global user.email "janesmith@acme.com"
      git config --global push.default simple

This will modify your *~/.gitconfig* file with those values

* Note: See git-config man page: Search *push.default* for more details

Clone a Repo
------------------------------------------------------------------------
To clone (download) a repository from the web:

* Find the repo online and get the *code* string.
* Copy that string. e.g., "git@github.com:acme/mousetrap.git"
* Pull a repo down with "git clone"

::

  [bash]: git clone git@github.com:acme/mousetrap.git

    - or for https -

  [bash]: git clone https://github.com/acme/mousetrap.git

Recommended Commit Message Format
---------------------------------

We regard the Git log as an important tool to track down and summarize problems
and solutions in our code. Therefore its critical that the commit messages
be as descriptive as possible. Remember that the summary line is the
one that carries a lot of weight and is only 50 characters or less.

.. note:: Here are the rules for commit message

   1. First line is Capitalized description, under 50 chars, no period
   2. Second line blank
   3. Third line: Fixes ACME-13123, ACME-34345, and ACME-99099
   4. Fourth Line blank
   5. Next lines all less than 72 charactors, elaborated description.
      It can be a bit more open.

Here is an ideal commit message::

  Correct the instance-table relations in the modeler

  Fixes ACME-123455.

  Here is a more description of the fix (but only if needed). Its wrapped
  to 72 chars to keep it readable.

If your commit is bigger and you need more bullets or description you
can still do it. For example::

  Correct the hyper-drive:

  Fixes ACME-123455, ACME-92548.

  We had to fix the relationships so that the original model was not
  modified. We still limit to 72 chars to avoid ugliness.
  I also added another tire to the system so that it would roll better.
  These are other things I have fixed:

  * Fixed model to keep old model intact
  * Added migration script to ensure model was correct at upgrade
  * Added a big fat tire for better rolling
  * Updated docs


Reasons for this Format
~~~~~~~~~~~~~~~~~~~~~~~~~

Why do we prefer this format? Partly because it allows a quick history view
with a command like::

   git log --decorate --graph --oneline --date-order

If the first line is just 'Fixes ACME-XYZ123' you get things that are not
helpful at all like::

   |/ /
   * |   c568cfe Merge pull request #93 from acme/feature/ACME-22568
   |\ \
   | * | a367040 (origin/feature/ACME-22568) fix ACME-22568
   |/ /
   * |   160f150 Merge pull request #99 from acme/feature/ACME-22599
   |\ \
   | * | f2d37aa (origin/feature/ACME-22599) fix ACME-22599

... instead of somthing useful like this::

   |/|
   * |   8f38040 Merge pull request #106 from acme/feature/ACME-22673
   |\ \
   | * | 58f9857 (origin/feature/ACME-22673) Add default values to properties; fix ACME-22673
   |/ /
   * |   f18a2ac (feature/ACME-22598) Merge pull request #99 from acme/feature/ACME-22639
   |\ \
   | * | 01d4868 (origin/feature/ACME-22639, feature/ACME-22639) Ensure modeling on Mars

... there are other reasons too. See references.

References:

* http://chris.beams.io/posts/git-commit/
* http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html

Cool Ways to Show Logs and Diffs
----------------------------------

Often you will need to see your logs and compare different versions::

   git log
   git log --oneline --graph --decorate --all
   git diff
   git diff fe492a1 #(between current and some other node)
   git diff be158f6 aee7163 #(between two nodes)

Typical Workflow Scenario
--------------------------------------------------------------

Now that you have a repo, go into the repo folder.

* Add any files you want
* Make any changes you want to files
* Commit your changes
* Push your changes

To add your new files::

  [bash]: git add -a abc.py def.py
  [bash]: git add -A (Danger: adds and removes files from working tree)

To commit all changes execute::

  [bash]: git commit -a

To finally push up your changes to your repo upstream::

  [bash]: git push

New Repo Workflow Scenario
--------------------------------------------------------------

* First go to GitHub/GitLab and create your account
* Then create an empty repository in the GUI
* Now on your workstation, pull down (clone) the empty repo::

  [bash]: git clone https://github.com/acme/bogus.git
  [bash]: cd bogus/

* Now start writing your code, make files.
* Now add files to your repo and push::

   [bash]: git add -A
   [bash]: git commit -a
   [bash]: git push
   [bash]: git status
   .. Already up-to-date ..

Changing Branches
-------------------------

* Change branch from **master** to **develop** with *checkout*::

  [bash]: git checkout develop
  [bash]: git status

Merging Branches
-------------------------

You like the work you've done in develop and think it should be merged into master.
You can do this by using the *merge* option.

* First change branches from develop to master::

  [bash]: git checkout master

Now things are as before with master in its original state.

* Now you want to merge from develop::

   [bash]: git merge develop
     Updating 1530600..2873dc4
     Fast-forward
     .gitignore                             |    2 +
     Makefile                               |   11 +-
     ...

* Now you must push these changes up to your Hub::

   [bash]: git push
     Total 0 (delta 0), reused 0 (delta 0)
     To git@github.com:acme/mousetrap.git
     1530600..2873dc4  master -> master


Delete Unwanted Branches
------------------------
If you want to eject unwanted branches from your repo,
make sure to read the git-branch docs and the warnings about being
fully merged (--delete option).

To remove a local branch::

  git branch -D <branchName>

To remove a  remote branch::

  git push origin --delete <branchName>


Synchronizing Local Branches and References: Pruning
-----------------------------------------------------
Sometimes you'll have a lot of old remote branch references that have
been long deleted on the hub. You can synchronize them with fetch::

    git fetch -p

Revert a Branch to a Prior Commit
--------------------------------------------------

*git revert* will create a new commit that will undo what the prior commit(s)
have done and put that into your history. It gives you a log of your undo.

Resetting a Branch to a Prior Commit
--------------------------------------------------
* git checkout feature/area51
* Identify the number of your last "good" commit::

    git log
    (grab the good commit number: e3f1e37)

* Reset your feature/area51 to that commit level::

    git reset --hard e3f1e37

* Push it up to GitHub/GitLab::

    git push --force origin feature/area51

* Test the diff between local and remote: Should show nothing::

    git diff feature/area51..origin/feature/area51


Comparison of Git Branches
---------------------------------------------------

* Show only relevant commits between two git refs::

   git log --no-merges master..develop


.. _avoiding_small_commits:

Avoiding Many Small Commits
---------------------------------------------------------------------
You can make as many small changes as you like and still have a clean
single commit by using git's amend flag on your commit::

    git commit --amend
    (make your commit message)
    (write/quit)

Every time you make a new commit in this way, you get the benefit of small
incremental changes and a clean commit log. If you have already made a mess
of things you can try the next technique to **Squash** your commits.


.. _squashing_multiple_commits:


Squashing Multiple Commits
---------------------------------------------------------------------
This allows you to take a lot of many small commits (and their messages)
and convert them to a single coherent commit. It keeps the history clean and clear.

In order to do this safely, we recommend only doing this in a feature branch
(based on develop) that is not being shared.

Version I: Squashing Against a Prior Commit
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Background: You created a branch and made some commits. You want to squash all
your small commits into a single unified commit, starting that the root of your
branch. The commit just BEFORE your changes is abc123def456.

* From your feature branch: If you have made several commits starting from (and
  not including) commit abc123def456, you can squash all your commits
  with a rebase::

      git rebase -i abc123def456

* When it shows you your commits::

      pick 01d1124 Adding Goods
      pick 6340aaa Moving my period
      pick ebfd367 Hyde has become self-aware.
      pick 30e0ccb Make my typo nice.

  edit this to become::

      pick 01d1124 Adding Goods
      squash 6340aaa Moving my period
      squash ebfd367 Hyde has become self-aware.
      squash 30e0ccb Make my typo nice.

* Now you write this out and to fix-up the commit logs in the next screen.
  Do this by changing to a single unified commit message::


      Feature ACME-1234: Adding Goods

      * Moving my period
      * Hyde has become self-aware.
      * Make my typo nice.

  then write it out.

* Now you have to force push, because now git is confused::

      git push -f

Version II: Squashing Against a Major Branch
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* From your feature branch, do a rebase with the -i flag::

    git rebase -i develop

* When it shows you the multiple commits, change command in commits after the first
  "pick" to "squash". Thus something like this::

      pick 01d1124 Adding license
      pick 6340aaa Moving license into its own file
      pick ebfd367 Jekyll has become self-aware.
      pick 30e0ccb Changed the tagline in the binary, too.

  now becomes::

      pick 01d1124 Adding license
      squash 6340aaa Moving license into its own file
      squash ebfd367 Jekyll has become self-aware.
      squash 30e0ccb Changed the tagline in the binary, too.


* Now you write that out and it will ask you to fix-up the commit logs.
  Do this by changing to a unified commit message::

    # This is a combination of 4 commits.
    # The first commit's message is:
    Dr Jekyll's final revisions to persona.

    - Add that license thing
    - Moving license into its own file
    - Jekyll has become self-aware.
    - Changed the tagline in the binary, too.

* Once you write that out, you need to push it up with force flag to rewrite
  history::

    git push -f

* If you have already pushed it up prior to this, or even created a Pull,
  your upstream commits and pulls will get replaced with the unified commit.


=============================================================================
Keeping History Clean
=============================================================================

Keeping the code history squeaky clean is critically important!

.. note::
   Its a common practice to review the git log history by using *git-log* and
   *git-blame*. This history is important and vital to understanding the
   evolution of the code and how to effectively modify it without repeating
   historical mistakes.

This is not as academic a point as it might seem. Almost every day we
dig through code while troubleshooting, and one of the most useful things
is finding the ticket related to a line of code.

We use git blame for this, and if the last time a line of interest was modified
(or lines around it) wasn’t functional, we have to find the parent of that
commit and re-run blame. We have to repeat this process for each non-functional
change. It’s damn annoying.

It also has the affect of making a section of code appear to be modified by many
authors over a long period of time,   which might indicate a troublesome area of
code, even though the code is exactly the same functionally as the first and
only time it was really written.

The following recommendations are made in order to maximize the
clarity of the history and blame records:

For new code: **Always** Check for Style Errors
--------------------------------------------------------------------
Getting style correct initially avoids cleanup or stylistic annoyances later.
You don't want to fix formatting/style errors later because they muddy the log
and blame history.

Reviewing Code: **Never** Make Style Corrections
---------------------------------------------------------------------
Minimizing stylistic changes for defects allows the diffs to isolate defect
changes alone. This allows the debug team to easily identify **significant**
changes that breaks functionality, and fix it.

If stylistic corrections are to be made, they should be done for:

* Code development
* Major cleanup
* Changes in the PR code itself

Don't make unrelated style fixes for defects.

Reviewing Code: Question Unrelated Cosmetic Changes
---------------------------------------------------------------------
If you are reviewing code for a defect, and you see style changes that are
**unrelated** to the bug or feature at hand, ask the author to revert those
changes so that the *git-blame* log will only reflect relevant changes to
defects or functionality.

Avoid Many Small Commits for Defects
---------------------------------------------------------------------
This doesn't mean that you should have super-huge 10000 line commits.
It does mean that:

0. Defects (and resulting PRs) should be minimal in scope to limit changes.
1. Small commits that are related should be put into the same commit. See above
   sections on how to :ref:`avoiding_small_commits` and
   :ref:`squashing_multiple_commits` .

Rebasing against Master (or any other branch)
---------------------------------------------------------------------
Its often desirable for you, the author, to rebase your branch against the
originating repo, because only you can prevent merge conflicts (forest fires).
This is important so that the GitHub/GitLab UI merges are clean and safe.
When the number of commits backs up, and someone is left to do these risky
merges and rebases, we can easily end up with hamburger
(see DeadPool for the gory details).

Instructions:

* Call your feature branch **feature/ACME-1234**

* From your feature branch, pull a clean master::

    git checkout master
    git pull
    get checkout feature/ACME-1234    # go back to your branch now.

* From your feature branch, do a rebase with the -i flag::

    git rebase -i master

* **Now if the rebase is clean, you are done. Stop.**

* If the rebase didn't go well, you have to resolve conflicts: For each conflict
  file:

  - Edit the file, fix the conflict as usual.
  - Add the file back to the commit pool::

        git add path/to/file

* Once all your conflicts are resolved as above, continue the rebase::

      git rebase --continue


  Note: you may have to go back to the previous step and resolve more issues if this fails.

* If you continue to have problems call the git police (ask for help in Slack).


* Now that your rebase is clean, push your feature back upstream::

    git push -f

* Now your PR should be clean and ready to merge. Go check it in GitHub/GitLab.


Placing your PR on Draft when you don't Want it Merged
---------------------------------------------------------------------
If you have a PR that is still in development, or you don't want that PR merged
for any reason, make sure to mark that PR as *Draft*.
Look for **Convert to Draft** in your PR.


Push the Develop Branch onto the old Feature that is Stale
----------------------------------------------------------

.. warning:: This flow can be dangerous. Use with caution!

You have created a branch (forgotten) that has been left behind and wish upgrade
it with all the new changes that have been made with other feature enhancements.
You don't have anything to save in it. Use these commands (with caution)
to merge develop back onto feature/forgotten::

  [bash]: git checkout feature/forgotten
  [bash]: git push . develop:feature/forgotten
  [bash]: get checkout feature/forgotten
  [bash]: git commit -a
  [bash]: git push

Push a new Feature up to Origin for storage:
-----------------------------------------------------
Sometimes you want a feature to be stored on your Hub.
Git-Flow does not automatically push your features.
You can push it up to the hub like this::

  [bash]: git push -u origin feature/new

Heavy Feature Workflow **IMPORTANT: PLEASE READ**
------------------------------------------------------

During heavy workflows on a project, we expect multiple
teams to concurrently work on multiple features that get merged into develop.
This is common. This heavy workflow can be managed by the following workflow:

* Create your feature/area51
* Work/commit/push to feature/area51 branch
* Another team does works on feature/Atlantis (don't be jealous!)
* Another teams merges feature/Atlantis into develop
* Now you need to rebase those changes into feature/area51 as follows::

   git checkout develop && git pull  # Get feature/Atlantis changes
   git checkout feature/area51
   git rebase develop

* If you have conflicts see `merge_conflicts`_ below, else continue
* Now continue work on feature/area51
* Repeat the rebases as needed when team Atlantis updates develop
* Once finished, create your final Pull Request
* Once merged, delete the feature branch.

.. _rebasing_feature:

Rebasing a Feature on Develop
------------------------------------------------------

.. WARNING::

   If your team is still working on a feature and you notice
   that develop has been updated, you should try to rebase
   those changes into your feature. This will avoid conflicts
   later when you merge back into develop.

You may get these messages

.. note::

    Branches 'develop' and 'origin/develop' have diverged.
    Fatal: And branch 'develop' may be fast-forwarded.

Someone has added to develop during your work on feature/area51.
This is common in a multi-user environment. You will
have to merge the two together. To solve this, you need to:

* Sync local develop with origin: checkout develop, pull from origin to
  develop::

    git checkout develop && git pull origin

* Rebase your feature on develop. You may have conflicts here if you're
  unlucky::

    git checkout feature/area51; git rebase develop

* Check that nothing is broken::

    git status
    git push    # Push feature/area51 up to origin

* If there are conflicts you have to fix here. See `merge_conflicts`_


.. _merge_conflicts:

Merge Conflicts: Fixing a Rebase
---------------------------------

If you do have conflicts with your merge you can take a simple approach
to fixing them:

* Rebase against develop::

    [joe@acme]: git rebase develop
      First, rewinding head to replay your work on top of it...
      Applying: Make Tenant rels concrete
      Using index info to reconstruct a base tree...
      M       Bricks/acme/Anvil/__init__.py
      Falling back to patching base and 3-way merge...
      Auto-merging Bricks/acme/Anvil/__init__.py
      Falling back to patching base and 3-way merge...
      Auto-merging Bricks/acme/Anvil/__init__.py
      CONFLICT (content): Merge conflict in Bricks/acme/Anvil/__init__.py
      Failed to merge in the changes.
      Patch failed at 0001 Make Tenant rels concrete
      The copy of the patch that failed is found in:
         /data/Bricks.acme.Anvil/.git/rebase-apply/patch

      When you have resolved this problem, run "git rebase --continue".
      If you prefer to skip this patch, run "git rebase --skip" instead.
      To check out the original branch and stop rebasing, run "git rebase --abort".

* Edit the problem file and fix::

   [joe@acme]: vi __init__.py
      ...fix stuff here...

   [joe@acme]: git status

      rebase in progress; onto 34ae002
      You are currently rebasing branch 'feature/ACME-17143_installWarnings' on '34ae002'.
      (fix conflicts and then run "git rebase --continue")
      (use "git rebase --skip" to skip this patch)
      (use "git rebase --abort" to check out the original branch)

      Unmerged paths:
      (use "git reset HEAD <file>..." to unstage)
      (use "git add <file>..." to mark resolution)

            both modified:   __init__.py


* Add this file back into to index::

   [joe@acme]: git add __init__.py

* Continue::

   [joe@acme]: git rebase --continue

      Applying: Make Tenant rels concrete
      Applying: fix context relations
      Using index info to reconstruct a base tree...
      M       Bricks/acme/Anvil/Tenant.py
      M       Bricks/acme/Anvil/__init__.py
      Falling back to patching base and 3-way merge...
      Auto-merging Bricks/acme/Anvil/__init__.py
      CONFLICT (content): Merge conflict in Bricks/acme/Anvil/__init__.py
      CONFLICT (modify/delete): Bricks/acme/Anvil/Tenant.py deleted in fix context relations and modified in HEAD. Version HEAD of Bricks/acme/Anvil/Tenant.
      py left in tree.
      Failed to merge in the changes.
      Patch failed at 0002 fix context relations
      The copy of the patch that failed is found in:
         /data/Bricks.acme.Anvil/.git/rebase-apply/patch

      When you have resolved this problem, run "git rebase --continue".
      If you prefer to skip this patch, run "git rebase --skip" instead.
      To check out the original branch and stop rebasing, run "git rebase --abort"

* Repeat: You may have to edit/re-edit a file, re-add, and continue as before::

   [joe@acme]: vi __init__,py
   [joe@acme]: git add __init__.py
   [joe@acme]: git rebase --continue
      Bricks/acme/Anvil/Tenant.py: needs merge
      You must edit all merge conflicts and then
      mark them as resolved using git add

* Delete what is required. You deleted a file but it is confused by this::

   [joe@acme]: git rm Tenant.py
      Bricks/acme/Anvil/Tenant.py: needs merge
      rm 'Bricks/acme/Anvil/Tenant.py'

   [joe@acme]: git rebase --continue
      Applying: fix context relations

* If at this point the merge is good, but it asks you to pull, don't pull!
  You really want to push your changes::

   [joe@acme]: git status
      On branch feature/ACME-17143_installWarnings
      Your branch and 'origin/feature/ACME-17143_installWarnings' have diverged,
      and have 14 and 2 different commits each, respectively.
      (use "git pull" to merge the remote branch into yours)
      nothing to commit, working directory clean

   [joe@acme]: git push
      To git@github.com:acme/Bricks.acme.Anvil.git
       ! [rejected]        feature/ACME-17143_installWarnings ->
       feature/ACME-17143_installWarnings (non-fast-forward)
       error: failed to push some refs to
       'git@github.com:acme/Bricks.acme.Anvil.git'
       hint: Updates were rejected because the tip of your current branch is
       behind
       hint: its remote counterpart. Integrate the remote changes (e.g.
       hint: 'git pull ...') before pushing again.
       hint: See the 'Note about fast-forwards' in 'git push --help' for details.

   [joe@acme]: git push --force
      Counting objects: 12, done.
      Delta compression using up to 8 threads.
      Compressing objects: 100% (12/12), done.
      Writing objects: 100% (12/12), 1.22 KiB | 0 bytes/s, done.
      Total 12 (delta 6), reused 0 (delta 0)
      To git@github.com:acme/Bricks.acme.Anvil.git
      + 2bfc0a6...f7ddee9 feature/ACME-17143_installWarnings ->
         feature/ACME-17143_installWarnings (forced update)

* If you see a clean status, its probably good. Make sure to test::

   [joe@acme]: git status
      On branch feature/ACME-17143_installWarnings
      Your branch is up-to-date with 'origin/feature/ACME-17143_installWarnings'.
      nothing to commit, working directory clean


Git Stash: Stashing Modified Files
------------------------------------

Git's *stash* option allows you to put modified files into a temporary holding
area. The usual scenario is to stash your mods away then pull from the origin,
and then re-place your stashed files into the tree. Then you can push the
results back up to origin. Here is a possible workflow::

  .... you made changes to develop, but you'd rather it be in a feature....

  [bash]: git stash
   > Saved working directory and index state WIP on develop: e38b798 post
   release: 1.0.1 -> 1.0.2dev.....

  [bash]: git checkout -b cleanup_on_aisle_7
   > Switched to a new branch 'feature/cleanup_on_aisle_7'

  [bash]: git stash pop
  .... now you have your new mods overlaid ....
  .... make whatever other modifications ....
  .... now you can commit all your mods ....

  [bash]: git commit -a

  [bash]: git push

Git Warnings and Errors
--------------------------------------

* You may you get this warning when trying to push a new branch to origin::

    [bash]: git push
    fatal: The current branch develop has no upstream branch.
    To push the current branch and set the remote as upstream, use

        git push --set-upstream origin develop

  Its usually safe to follow this suggestions

References
-----------
* https://git-scm.com/book/en/v2


