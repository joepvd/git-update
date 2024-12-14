==========
git update
==========

-----------------------------------------
Do the right thing when updating branches
-----------------------------------------

:Author: joepvd
:Date: 2024-12-14
:Copyright: GPLv2
:Version: 0.2
:Manual section: 1

SYNOPSIS
========

git update


DESCRIPTION
===========

For automatically rebasing current branch to freshly fetched origin branch, and rebase local branches with upstream changes.
Optionally, a configuration file can be used for a list of read-only checkouts for relevant branches.


If you're daring:
```sh
git config --global alias.pull git-update
```

Features (any repo)
-------------------

* fetches upstream origin
* ensures the local main branch is current with the upstream main branch
* ensures the currently checked out branch gets the changes from its originating branch

Features configured repo
------------------------

In addition to the general features, it is possible to configure read only checkouts in
a different directory. Take this example:

.. code-block::
   - repo: ~/src/github.com/openshift-eng/ocp-build-data
     worktree_dir: ~/ocp-ro
     checkouts:
     - branch: openshift-4.18
       dir: "4.18"
     - branch: openshift-4.19
       dir: "4.19"



It:
* ensures all `openshift-$VERSION` branches are up to date with their origin
  counterpart
* ensures the read-only worktree checkouts under `~/ocp-ro/$VERSION` are up to
  date with origin.
* can run `git update` in read only worktree, and updates the real repository
  with all relevant branches, and all the read only worktree checkouts.

Example config is in ``/usr/share/git-update/git-update.yaml``. Copy to ``~/.config/git-update.yaml``
and edit.

This makes use of the [worktree](https://git-scm.com/docs/git-worktree) feature of `git`.
The result is a directory `~/ocp-ro`, where for each relevant version, a complete and
current checkout of the origin state of that version is.

The purpose of these checkouts is to easily query and compare facts between
branches.

These checkouts are headless, as the local knowledge of the remote state is
checked out. Files and directories in the worktree checkouts are set to be
readonly, to ensure the character of "upstream truth" is maintained.
