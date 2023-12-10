# git update

For automatically rebasing current branch to freshly fetched origin branch, and rebase local branches with upstream changes.

## Usage:
```sh
git update
```

## Installation
```sh
git clone https://gist.github.com/35ec4a4b77b7f3321f48ae279c78c63f.git git-update
ln -sf "$(pwd)/git-update/git-update" ~/bin/git-update
```

If you're daring:
```sh
git config --global alias.pull git-update
```

## Features (any repo)
- fetches upstream origin
- ensures the local main branch is current with the upstream main branch
- ensures the currently checked out branch gets the changes from origin/main in
- 

## Features for ocp-build-data
In addition to ☝ general features, `ocp-build-data` is treated in a special way.
It:
- ensures all `openshift-$VERSION` branches are up to date with their origin
  counterpart
- ensures the read-only worktree checkouts under `~/ocp-ro/$VERSION` are up to
  date with origin.
- can run `git update` in read only worktree, and updates the real repository
  with all relevant branches, and all the read only worktree checkouts.

## Wait what. "Read only worktree checkouts"?!?
Yes. Consider this a daemonless successor to [gitfuse](https://github.com/joepvd/gitfuse).

This makes use of the [worktree](https://git-scm.com/docs/git-worktree) feature of `git`. The result is a directory `~/ocp-ro`, where for each relevant version, a complete and current checkout of the origin state of that version is.

The purpose of these checkouts is to easily query and compare facts between
branches.

These checkouts are headless, as the local knowledge of the remote state is
checked out. Files and directories in the worktree checkouts are set to be
readonly, to ensure the character of "upstream truth" is maintained.

## Bugs
- Config values are hard coded
- ocp-build-data is hard coded
- worktree is global and hard coded
- If current branch equals one of the versioned special branches, it will get
  evaluated for rebase twice
