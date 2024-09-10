.. _releases:

=====================================
Releases
=====================================

Most organizations have a formal process for releasing Software.
The general steps are:

* 3rd Party Software approval, if required
* Ensure an appropriate copyright is included in each source file
* Create, modify, and merge the *release* branch
* Inform the Release Team
* Deploy the new version
* Publish the Documentation on Wiki

We only cover the technical steps related to code handling here.

Appropriate copyright for each Source File
============================================

Attach the appropriate copyright notice.

Find the files that are missing::

   # find . -size +1 -regex ".*\.\(rs\|go\|sh\|py\)" -exec grep -iL 'copyright' {} \;

Release Feature Branch
===============================
The release feature branch is similar to a hotfix, except that:

a. It comes from *develop*
b. version numbers are bumped, and 
c. release notes are added

:: 

    ========*================> main branch
           /
          @====X  (deleted)    release branch
         / \
    ====*===*================> develop branch


Set the Version
----------------

First set the releast version:

* We recommend using the format: Major.Minor.Revision: e.g.: 1.2.3
* Version numbers typically starting at 1.0.0.

  - The first digit is the major version
  - The second is the minor version
  - The third is the revision.

Set the following environment variables as per your situation and adjust the
version numbers:

* For a revision, maintenance, or a hotfix, increment the Revision number for
  *main* and *develop*. e.g ::

   export OLD=3.1.6       # The previous Main release
   export CURRENT=3.1.7   # The current Develop -> new Main release
   export NEW=3.1.8       # The new develop after release

* For a Minor release, we bump the minor number for *main* and *develop*.
  Note that the revision number goes to zero in *CURRENT*. ::

   export OLD=3.1.6       # The previous Main release
   export CURRENT=3.2.0   # The current Develop -> new Main release
   export NEW=3.2.1       # The new Develop branch after release; bump Minor

* For a Major release, we bump the major number *main*, and minor for develp.
  e.g::

   export OLD=3.1.6       # The previous Main release
   export CURRENT=4.0.0   # The current Develop -> new Main release
   export NEW=4.0.1       # The new Develop branch after release

Start the Release Process
----------------------------------
Starting from a clean *develop* branch::

   git checkout develop
   git status
   # (ensure develop is in a clean state)

* Start the Release::

    git checkout -b release/1.0.0 main

* Update the documentation:

   - The **Changes** section
   - Essential functionality that has changed
   - Breaking changes, if any
   - Clearly list any relevant defect fixes by number

* Edit version info

   - Remove the "dev" in the version number if any
   - Ensure that the version is the $CURRENT value in code

* Commit:

   - For new release (brand new repo)::

      git commit -am "release: version $CURRENT"

   - For update release::

      git commit -am "release: version ${OLD} -> ${CURRENT}"


* Finish the release::

   # merge changes into the main branch
   git checkout main
   git merge --no-ff release/$CURRENT
   git tag -a v$NEW -m "Release $NEW"
   git push origin v$NEW  # This pushes the tag too

   # merge changes into the develop branch
   git checkout develop
   git merge release/$CURRENT
   git push origin develop

   # remove the release branch
   git branch -d release/$CURRENT

* Commit again::

    git commit -am "post release: $CURRENT -> ${NEW}"

* Push branches and tags::

    git push --all
    git push --tags
    

.. _release_stage_develop:

Stage Develop to the Next Version
------------------------------------

Make sure you bump develop to either the next Major or Minor release as per required.
In our case, using $OLD, $CURRENT, and $NEW::

   # Make the basic changes to develop
   git checkout develop
   git checkout -b develop/$NEW
   # Update the version numbers wherever you need to in code/docs
   git commit -am "Bump version to $NEW"

   # Merge the changes to develop and push
   git checkout develop
   git merge --no-ff develop/$NEW
   git push origin develop

   # Cleanup
   git branch -d develop/$NEW


.. _clean_git_repos:

Clean Git Repo Branches
--------------------------
Having abandon or inactive Git repos can lead to unintended merges or history rewrites.
Once you finish with a feature, hotfix, or temporary branches, remove them.

* Remove the feature branch as soon as you merge it.
* Remove any old and unused Git feature/hotfix branches.
* Make sure that feature/hotfix branches are merged prior to removal.
* Make sure that unfamiliar feature/hotfix branchs are not active prior to removal.

.. _release_publishing:

Deployment Process
==============================

For your organization, you may have to jump through other hoops.
Consult your team for details.

Publish the Official Documentation
-----------------------------------

Update the public facing documentation. At the bare minimum:

* Update your docs as per your corrections in section `Start the Release Process`_ :

  - Make sure to update essential documentation that has changed
  - Make sure to update the **Release** date and data
  - Make sure to update the **Changes** section 


Quick Fixes to Documentation
=====================================================

.. NOTE:: Main should remain identical to the most recently tagged version.

In general, don't modify *main* directly because it will diverge the *main* branch
from the release tag that represents the current release. Instead we modify a
hotfix branch that gets merged into *develop* and eventually into *main*:

0. You have released and tagged a new *main*.
   You realize you need to make some quick doc fixes.
1. Create the next hotfix branch.
2. Update the docs in that hotfix branch.
3. Merge that hotfix branch into *develop*.

If you can't use a hotfix model, just put the changes into *develop*.
