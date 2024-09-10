.. _gitflow:

=============================================================================
Git-Flow (Deprecated)
=============================================================================

We don't recommend Git-Flow any more. Instead we recommend GitHub flow.
See: https://www.geeksforgeeks.org/git-flow-vs-github-flow/

Git flow simplifies development flow cycle.
See http://danielkummer.github.io/git-flow-cheatsheet/

.. _gitflow_setup:

Installing Git-Flow
------------------------------------

.. note:: There are two versions of Git-Flow: NVIE and AVH (newer).
          Although both versions have the same basic commands, some of the
          commands in AHV are easier to use than older NVIE. The NVIE version
          has not been active since 2012.

       For this reason we recommend using the active AVH version.

Git flow installation is well documented and simple.
Refer to the following links for installation instructions:

* AVH: https://github.com/petervanderdoes/gitflow-avh

Setup Git-Flow in the Existing Repo
------------------------------------
First go into the repo base folder. Make sure you get a clean *git status*.
Then you initialize the git repo for *git flow* as follows:

::

   [bash]: git flow init

      Which branch should be used for bringing forth production releases?
         - develop
      Branch name for production releases: [] master

      Which branch should be used for integration of the "next release"?
         - develop
      Branch name for "next release" development: [develop]

      How to name your supporting branch prefixes?
      Feature branches? [feature/]
      Release branches? [release/]
      Hotfix branches? [hotfix/]
      Support branches? [support/]
      Version tag prefix? []
      Hooks and filters directory? [/data/Bricks.acme.Anvil/.git/hooks]

Create New Features and Work Flow
----------------------------------
In features, you don't want to use version numbers because it can
cause chaos when multiple authors work the same project. Instead
give the version a name, and only after the resulting develop is
reviewed, you give it a version. (Source Unknown: Rob B).

To start a new feature::

    [bash]: git flow feature start area51
    [bash]: git flow feature publish
      - (This creates the feature branch on Github, and allows "push")
    [bash]: git status
        On branch feature/area51
        nothing to commit (working directory clean)

        .... do some work ....
        .... do some more work ....
        .... you are finished ....
        .... now commit ....

    [bash]: git commit -am "Comment: This fixes bug ACME-3234823"
    [bash]: git push (nothing happens)
      . Counting objects: 4, done.
      . Writing objects: 100% (4/4), 647 bytes | 0 bytes/s, done.
      . Total 4 (delta 3), reused 0 (delta 0)
      . To git@github.com:acme/Anvil.git
           6f1c83e..faf56f5  feature/area51 -> feature/area51

      - (Later you ask for a Pull Request or continue modifications)
      - (Someone may merge your Pull req into develop)
      - (Now you are finished with this feature...)
      - (You can either delete this branch or git-flow finish it)

    [bash]: git flow feature finish area51
    [bash]: git status
     On branch develop
     nothing to commit (working directory clean)

Now you are back on develop. You still need to push your changes up::

  [bash]: git push
   Total 0 (delta 0), reused 0 (delta 0)
   To git@github.com:acme/mousetrap.git
   1530600..2873dc4  develop -> develop

