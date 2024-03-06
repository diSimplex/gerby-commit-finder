# Commit History

This directory contains 'commit history' used to identify the commits to
the
[Gerby-project/PlasTeX@gerby](https://github.com/gerby-project/plastex)
("Gerby-PlasTeX") and
[PlasTeX/PlasTex@master](https://github.com/plastex/plastex) branches.

This directory has an analysis of the commit history of both the PlasTeX
(@master) and Gerby-PlasTeX (@gerby).

To do this we obtained the "raw commit history" of both PlasTeX ang
Gerby-PlasTeX repositories. These two files are contained in the
`rawCommitHistory` subdirectory as:

- `gerby-plastex-gerby.rawCommits` : All of the commits in the current
   (commit: `e1a7518` on 20/11/23) `gerby` branch of the
   Gerby-project/PlasTeX repository, in 'one-line' format.

- `plastex-plastex-master.rawCommits` : All of the commits in the current
   (commit: `4b66cd7` on 13/02/24) `master` branch of the PlasTeX/PlasTeX
   repository, in 'one-line' format.

We then used `meld` to identify the common commit history, as well as the
commit histories for each repository individually. These files are
contained in this directory:

- `common-base.commits` : All of the commits *before* the Gerby-PlasTeX
  fork.

- `common-group-1.commits` : A sequence of commits from PlasTeX which were
  pulled back into the Gerby-PlasTeX fork.

- `common-group-2.commits` : Another sequence of commits from PlasTeX
  which were pulled back into the Gerby-PlasTeX fork.

- `gerby-plastex-gerby-group-1.commits` : The first sequence of commits
  which are unique to the Gerby-PlasTex fork (between
  `common-base.commits` and `common-group-1.commits`).

- `gerby-plastex-gerby-group-2.commits` : The second sequence of commits
  which are unique to the Gerby-PlasTeX fork (between
  `common-group-1.commits` and `common-group-2.commits`).

- `gerby-plastex-gerby-group-3.commits` : The last sequence of commits
  which are unique to the Gerby-PlasTeX fork (after
  `common-group-2.commits`).

- `gerby-plastex-gerby-group-all.commits` : All of the commits which are
  unique to the Gerby-PlasTeX fork.

- `plastex-plastex-master-group-1.commits` : The first sequence of commits
  which are unique to the PlasTeX repository (after the
  `common-group-2.commits`).

- `plastex-plastex-master-group-all.commits` : All of the commits to the
  PlasTeX repository after the `common-base.commits`.

The script `re-assemble` provides a definitive test of which sequence of
commits belongs to which repository.
