===============
Pull Requests
===============

Pull requests are the primary way we get code changes from a feature/bug branch into
one of our big branches like main, develop, or feature/area51. They usually go
through a review process to make sure they are defect free.

Pull Requests via GUI
========================

The easiest way we have to get your code reviewed and merged into a major branch
is to create a feature, push that feature up to GitSmeg, and have someone review
it. Here is the workflow:

* Create your new feature branch
* Make your mods
* Commit your mods
* Push (or publish) your feature up to Origin
* Go into the GitSmeg GUI, select your feature
* Make your pull request (against main, develop, or feature/area51)
* Ask for a review, and get it accepted. Make revisions based on review
* Then merge your changes into develop, removing the feature branch
* Finally, from original branch, do a *git pull* to sync up

Pull Requests via CLI
==========================

Everything is similar to the above sequence except you need to install one or
both of the CLI tools for the different repository services, Github and Gitlab:

* gh for Github: https://github.com/cli/cli
* glab for Gitlab: https://gitlab.com/gitlab-org/cli

Creating a PR on CLI
---------------------
These two commands will guide you through the process to create a PR:

For Github::

   gh pr create -b <base_branch>

For Gitlab::

   glab mr create --fill -b <base_branch>

Merge a PR on CLI
-------------------

For Github::

   gh pr merge <url>

For Gitlab::

   glab mr merge <number>

Review a PR on CLI
----------------------

For Github::

   gh pr review [url]

For Gitlab you should use the Gitlab website


