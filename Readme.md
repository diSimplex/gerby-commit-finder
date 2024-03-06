# Gerby commit finder

A simple collection of Python scripts to identify changes made by
[Gerby](https://gerby-project.github.io/) to the base
[PlasTeX](https://plastex.github.io/plastex/) [code
base](https://github.com/plastex/plastex).

## The problem

The objective of this repository is to analyse the
[Gerby-project/PlasTeX](https://github.com/gerby-project/plastex) and
[PlasTeX](https://github.com/plastex/plastex) repositories to identify
those Gerby-Project/PlasTeX commits which *change code* in the base
PlasTeX code base.

This helps identify which commits might need to be back-ported to the main
PlasTeX repository. (See [PlasTeX issue #160 : Allow Gerby to use upstream
plasTeX](https://github.com/plastex/plastex/issues/160))

At the moment (03/March/2024), the "official" `gerby` branch of
[Gerby-Project/PlasTeX](https://github.com/gerby-project/plastex) (commit:
`e1a7518` on 20/11/23) is 143 commits ahead of, and 480 commits behind,
the `master` branch of [PlasTeX/PlasTeX](plastex/plastex) (commit:
`4b66cd7` on 13/02/24).

**In fact** given [PlasTeX's plugin system]() the real question is which
of the Gerby project's changes to PlasTeX's base code actually *MUST* be
implemented by changes to the base code and which could be implemented by
an Gerby plugin as a "layer" over top of the PlasTeX's existing code.

This suggests for each of the Gerby project's commits we need to identify
a number of things:

1. Does the commit change a `Renderers` file? If so, then can this change
   be applied to the corresponding file in the Gerby plugin?

2. Does the commit change a `Packages` file? If so, then can this change
   be applied to the corresponding file in the the Gerby plugin?

3. Does the commit change a "Base" files?

   (Any thing which is not in either of the `Renderers` or `Packages`
   directories is considered a "Base" file).

   If so, is this change *required* to provide a working Gerby plugin?

   Alternatively, does this change reflect an over-sight in the PlasTeX
   code base for functionality which *is* in one or more TeX or LaTeX
   packages on CTAN?


## The analysis

In order to answer these questions, we have:

1. Obtained the commit history of both repositories (see the
   `commitHistory` directory).

2. Grabbed the difference file for each commit which are unique to
   the Gerby-PlasTeX fork, using the `tools/grabCommit` tool. These
   difference files are organized by both the PlasTeX files they change
   (see the `filesChanged` directory), as well as by the commit (see the
   `commits` directory).

   In all cases, in this analysis, any given "commit" is identified by
   `YYYY_MM_DD-HH_MM_SS-comitID`.

3. Conducted a test application of these differences by using the
   `tools/testApplyDiffs` tool which (recursively) walked over the
   difference files contained in the `filesChanged` directory, and applied
   (using the `patch` tool) each difference file to the appropriate
   PlasTeX file.

   Many of these applications resulted in `patch` "failures". For each
   such "failure" the differences were applied by hand to the appropriate
   PlasTeX file, and a "hand patch" was created using the
   `tools/createHandPatch` tool (see the `handPatches` directory).

   Some differences were determined to be un-needed and so "ignored". (Any
   entry in the `patchUps.yaml` file which does not include *either*
   `patch:` or `patchOK:` are *ignored*).

   The full analysis of what was done at this stage can be found in the
   `details:` entry for each changed file as recorded in the
   `patchUps.yaml` file.

4. Using the decisions recorded in the `patchUps.yaml` file, we then used
   the `tools/testApplyDiffs` again to create a version of the PlasTeX
   code base which contained the proposed changes.

   We then used `tox -e py311 -v -v -v` to run *all* pytests against this
   changed code base.

   For each pytest failure, we have written an analysis of the failure in
   Markdown format in the `pytestFailures` directory.

## Questions

**Q** At the moment, PlasTeX plugins can only over-lay "Renderers" and
"Packages". Is it possible to extend the PlasTeX plugin system to allow a
PlasTeX plugin to "over-lay" functionality in the "Base" ("LaTeX" or
"TeX") code?

**Q** Alternatively, could some of the existing code in the PlasTeX "Base"
be moved to a package in the "Packages" code?

**Q** As yet another alternative, should there be a "Gerby" LaTeX package
which implements some of the commands and environments which the existing
Gerby code alters in the PlasTeX's "Base" code?
