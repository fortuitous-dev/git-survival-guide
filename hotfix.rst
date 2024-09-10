.. _hotfix_release_notes:

========================================================================
HotFix Releases
========================================================================
Generated: |today|

The hotfix process is used often in software development. Its purpose is to
allow critical fixes to be made to the *main* branch. When a customer finds a
critical bug that has to get fixed fast, the hotfix is a good solution.

The hotfix process starts from main, and is merged back into main, but also merged
into develop to keep that branch consistent. Once the hotfix mission is
completed, it is destroyed as per the following diagram:

:: 

    ====*===*================> main
         \ /
          @====X hotfix
           \
    ====*===*================> develop


* Here we use a 3 digit versioning system: Major.Minor.Revision, EX: 2.4.5
* Develop branch already differs from main by a Major or Minor number

.. Warning:: Do not release a hotfix until QA has tested it.

Preliminaries
==========================

First lets get all the version numbers in order. We'll make bash
variables for these.

* Assume that develop is *already* incremented by a Major or Minor revision.
* We use the following bash variables to reference our version numbers:
  
    export OLD_PROD=2.2.0
    export NEW_PROD=2.2.1
    export DEVELOP=2.3.0

Hotfix Workflow
===============================================================================

The hotfix process is as follows:

#. git checkout main
#. git pull
#. git checkout -b hofix/$NEW_PROD main
#. Edit any code that references versions: change $OLD_PROD -> $NEW_PROD
#. git commit -am "Start $NEW_PROD Hotfix: Version $OLD_PROD -> $NEW_PROD"
#. Create a PR:

   - Squash all small commits for clarity
   - Ensure all tests pass
   - Ensure QA passes (if any)

#. Finish the hotfix:

   - git checkout main
   - git merge --squash hotfix/$NEW_PROD
   - git push origin main
   - git branch -d hotfix/2.2.1

Using the *gh* CLI for HOTFIX
==================================
The *gh* command can make the hotfix workflow a bit easier. Here is how:

* git checkout -b hotfix/$NEW_PROD main
* Make needed changes to your code: version numbers and fixes
* git commit -am "Hotfix: Fix critical issue in version $OLD_PROD"
* Create a PR (creates pr #1234) :: 

    gh pr create  \
       --title "Hotfix: Fix ACME-911 in $OLD_PROD" \
       --base main --head hotfix/$NEW_PROD

       >> Created pull request #1234

* Get approval for your pr #1234
* gh pr merge 1234

Commit Changes to Hotfix (deprecated):
===============================================================================

Here follow a normal defect-fix process but adapted to be based on the hotfix.
Once defects are made and hotfix is finished, hotfix will get merged into both
main and develop branches. Details ensue:


#. Make and commit changes needed in $NEW_PROD hotfix

    a) git flow feature start ACME-XXX hotfix/$NEW_PROD
    b) git flow feature publish
    c) **work, work, commit & push**
    d) **make pull request from feature/ACME-XXX to hotfix/$NEW_PROD**
    e) **Ensure small commits are squashed: :ref:`squashing_multiple_commits`**
    f) **review and merge**
    g) git checkout hotfix/$NEW_PROD
    h) git pull
    i) **repeat for all other existing issues**

#. Make sure to clean up all features branches that are merged: 
   :ref:`clean_git_repos`

Finish hotfix when all fixes are made:
===============================================================================

Now all hotfix changes are made and you are ready to merge into main
(and implicitly, develop).

.. warning:: QA must test and approve the hotfix branch before you perform the
             folloing steps. Make sure you have approval before releasing.

#. **UNIT TESTS MUST PASS**
#. **QA has tested and approved all defects, or you have higher approval**
#. **Update Release section**
#. **Update Changes section**
#. git commit -am "Finish $NEW_PROD Hotfix: Version $OLD_PROD -> $NEW_PROD"
#. git push
#. git flow hotfix finish $NEW_PROD
    - When asked for the tag, use::
      
       tag $NEW_PROD

#. **On the entry page specify: tag $NEW_PROD**
#. **You will be automatically merged into develop**
#. More than likely you will see a merge conflict with setup.py on the develop branch
    a) **fix the version in setup.py to match $DEVELOP**
    b) git add setup.py
    c) git commit -a  # Remove any conflict log entry, assuming you resolved it
    d) git push
    e) **fix any other conflicts, commit, and push**
    f) git flow hotfix finish $NEW_PROD
#. git push origin main
#. git push origin develop
#. git push --tags

Sanity Check
===============================================================================

You should now be in the develop branch.
Make sure all branch logs look like what you expect them to be.
At the very least, make sure that:

* git status on all branches is clean
* main logs have the hotfix changes
* develop logs have the hotfix changes
* The main and develop diff has no hotfix changes::

   git diff main..develop

Here are some other (undocumented) commands that may be useful:

* git status
* git log --all -p --graph
* git diff main --stat
* git checkout main
* git log --graph
* git checkout -          # (returns to develop branch)

Build the Master on Jenkins
===============================================================================

Now run a build of the main job if you have that workflow,
and verify that the artifact has the new $NEW_PROD version.


Workflow: Easy CLI Pull Requests on a Hotfix Branch
===============================================================================

If you have *hub* from https://github.com/github/hub and hub is aliased to
git, you can do the following in the CLI to make pull requests::

   git flow hotfix start 1.2.3
   git flow hotfix publish 1.2.3
   git flow feature start ACME-54321 hotfix/1.2.3
   git flow feature publish ACME-54321
   # work, work, work commit & push
   git pull-request -o -b hotfix/1.2.3

   
