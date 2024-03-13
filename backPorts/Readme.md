# The backports

We base our analysis of the required back-ports, from Gerby-PlasTeX back
into the PlasTeX code base, using two sources:

1. The commits suggested in @dalcde's first comment to the [Allow Gerby to
   use upstream plasTeX · PlasTeX Issue #160 ·
   plastex/plastex](https://github.com/plastex/plastex/issues/160) (see
   below).

2. The definitive list of commits (as of early March 2024) from both the
   [gerby-project/plastex](https://github.com/gerby-project/plastex) and
   the [plastex/plastex](https://github.com/plastex/plastex) repositories
   as analysed in this repository.

   This analysis is effectively summarised in the `patchUps.yaml` file,
   and tested using the existing PlasTeX PyTests contained in the
   PlasTeX's `unittests` directory.

   The resulting working patches can be found in the `handPatches`
   directory of this repository, organised by each file contained in the
   PlasTeX code base.

   We *only* consider changes to the PlasTeX "base code". That is any file
   *outside* of the PlasTeX `Packages` or `Renderers` directories. The
   non-base-code changes *should* be placed into a separate [Gerby
   plugin](https://github.com/diSimplex/plastex-gerby-plugin).

The analysis in this directory needs to ensure each of @dalcde's suggested
commits as well as any additional base-changing commits discovered in this
repository's analysis, are considered for back-ports into the PlasTeX code
base.

**Any such back-port *MUST* make only simple well-contained changes which
are *required* for the operation of the Gerby plugin.**

The final definitive test of these back-ports (and associated Gerby
Plugin) will be the "Stacks-test". This "Stacks-test" is defined as a
(nearly) perfect match of a large percentage of the [Stacks
document](https://stacks.math.columbia.edu/) tags, as they exist in early
March 2024, compared to a local rendering of the [Stacks
repository](https://github.com/stacks/stacks-project) using the
back-ported PlasTeX together with a new Gerby Plugin. This test can be
automated using the
[diSimplex/gerby-tester](https://github.com/diSimplex/gerby-tester).

## Analysis

### @dalcde's list

@dalcde's list is organised by commit id (at the moment we *only* consider
changes to the base code):

- Implemented the marginnote package
  [1339535](https://github.com/plastex/plastex/commit/13395353a85bf54034db9e07d1ce5132ebf6e7f4)
  - has already been fixed in PlasTeX

- Implemented output propagation
  [7f7fa2b](https://github.com/plastex/plastex/commit/7f7fa2b88d437243a4f7036bbd3be0962f042ffc)
  - changes `plasTeX/Renderers/__init__.py`
  - existing patch passes local PyTests

- Fixes [Unhandled mathematics in 0BM6: handle accents inside text mode
  inside math
  gerby-project/plastex#4](https://github.com/gerby-project/plastex/issues/4)
  [4176b6a](https://github.com/plastex/plastex/commit/4176b6afe48eb9379b6c5aea09064c70bacd0cb6)
  - changes `plasTeX/Base/LaTeX/Accents.py`
  - `handPatches/plasTeX/Base/LaTeX/Accents.py` passes local PyTests
  - requires `handPatches/unittests/accents.py` to correct PyTest

- Make sure the EndRow argument for matrix environments leaves initial …
  [8f98825](https://github.com/plastex/plastex/commit/8f9882513c6dd88331709ccce3f858d121740579)
  - changes `plasTeX/Base/LaTeX/Arrays.py`
  - changes `plasTeX/__init__.py`
  - `handPatches/plasTeX/Base/LaTeX/Arrays.py.diff` passes local PyTests
  - requires `handPatches/unittests/benchmarks/align.html.diff` to correct PyTest
  - `handPatches/plasTeX/__init__.py.diff` passes local PyTests

- Removing custom invoke on index
  [bf4a617](https://github.com/plastex/plastex/commit/bf4a617cd0a4eb312599f6b7d8051a6e0b808697)
  - changes `plasTeX/Base/LaTeX/Index.py`
  - this patch makes far to radical a change to the PlasTeX base code
  - see `pytestFailures/solutions/removingInvokeInLatexIndex.md`
  - we do not currently apply this patch
  - ***This decision requires Stacks-testing***

- Dealing with references
  [6572b72](https://github.com/plastex/plastex/commit/6572b726c8f9df09b8e390745f31093c703fa1de)
  - changes `plasTeX/Base/LaTeX/Quotations.py`
  - patched as part of `handPatches/plasTeX/Base/LaTeX/Quotations.py.diff`
  - passes local PyTests
  - ***Is this patch needed here or could it be put into the Gerby plugin?***

- Getting rid of extra whitespace in citations
  [495d0fb](https://github.com/plastex/plastex/commit/495d0fb717cfaa4560702578a43d4995eb3b2a40)
  - This is a change to the `plasTeX/Renederers/Gerby`
  - It should be addressed as part of the Gerby plugin

- Add meta information into Context; in particular, line count.
  [eae2c93](https://github.com/plastex/plastex/commit/eae2c937ad18b12127553ec22ce46a43344e4be7)
  - changes `plasTeX/Context.py`
  - changes `plasTeX/Tokenizer.py`
  - patch passes local PyTests

- Add dictionary that keeps track of individual arguments.
  [4b36f93](https://github.com/plastex/plastex/commit/4b36f938e93a1c283e1f886e7c1b312bcb070422)
  - changes `plasTeX/__init__.py`
  - `handPatches/plasTeX/__init__.py.diff` passes local PyTests

**Charsub related:**

These commits really need to be considered as a whole as they often do
something which is the undone in a subsequent commit.

These commits are best considered by reviewing the following the accepted
changes to the files: `plasTeX/__init__.py`, `plasTeX/Base/LaTeX/Math.py`,
`plasTeX/Base/LaTeX/Verbatim.py`, and `plasTeX/DOM/__init__.py`.

- New Command class so arguments have no charsub.
  [8498571](https://github.com/plastex/plastex/commit/84985719496a5e0e47d01f9607073450e5278a0e)
  - changes `plasTeX/__init__.py`
  - see `handPatches/plasTeX/__init__.py.diff` (which passes local PyTests)
  - was unwound by `2018_02_20-15_33_38-b65433a`
  - the `NoCharSubCommand` class is not currently used in the Gerby-PlasTeX code base

- No character substitutions in math environments.
  [bcb2524](https://github.com/plastex/plastex/commit/bcb2524460d9cfe893ade92619ef940316dbec18)
  - changes `plasTeX/Base/LaTeX/Verbatim.py` (which has been unwound and/or implemented by PlasTeX)
  - changes `plasTeX/Base/LaTeX/Math.py`
  - changes `plasTeX/__init__.py`
  - see `handPatches/plasTeX/Base/LaTeX/Math.py.diff` (passes local PyTests)
  - see `handPatches/plasTeX/__init__.py.diff` (which passes local PyTests)
  - the removal of the `invoke` method was unwound by `2019_12_21-14_25_46-b0f07ee`
  - addition of the `normalise` method was unwound by `2018_02_20-15_33_38-b65433a`

- Refactor the character substitution to a flag.
  [b65433a](https://github.com/plastex/plastex/commit/b65433a07dc12408c17e40aeb53a93cf75b9d38d)
  - changes `plasTeX/Base/LaTeX/Verbatim.py` (has been unwound and/or fixed by PlasTeX)
  - changes `plasTeX/Base/LaTeX/Math.py`
  - changes `plasTeX/__init__.py`
  - see `handPatches/plasTeX/Base/LaTeX/Math.py.diff` (passes local PyTests)
  - see `handPatches/plasTeX/__init__.py.diff` (which passes local PyTests)
  - essentially unwinds `2017_09_10-00_17_40-8498571` and `2018_02_11-18_32_48-bcb2524`

- Further things that should not do charSubs.
  [1a21e0c](https://github.com/plastex/plastex/commit/1a21e0c7e39db1ea9ce8ab97eaf96cf12566093c)
  - changs `plasTeX/Base/LaTeX/Math.py`
  - see handPatches/plasTeX/Base/LaTeX/Math.py.diff (which passes local PyTests)

- No character substitutions in math mode.
  [3722774](https://github.com/plastex/plastex/commit/3722774f712dbdacf17b33013188e9d8cc1557cd)
  - changes `plasTeX/DOM/__init__.py`
  - changes `plasTeX/__init__.py`
  - see handPatches/plasTeX/\_\_init\_\_.py.diff (which passes local PyTests)
  - see handPatches/plasTeX/DOM/\_\_init\_\_.py.diff (which passes local Pytests)
  - unwinds `2018_02_20-15_33_38-b65433a`

The following commit involve changes to the `charsubs` handling in one way
or another but was not included in @dalcde's original list:
`2019_12_21-14_25_46-b0f07ee`. This commit *has* been included in this
repository's analysis.

**Pending PR:**

- Fixes [Don't render apostrophes
  gerby-project/plastex#37](https://github.com/gerby-project/plastex/issues/37)
  [a0abec7](https://github.com/plastex/plastex/commit/a0abec7d0ab5982ab2a77296f6cca3b028b3af44)
  ([Add option disable some charsubs
  #185](https://github.com/plastex/plastex/pull/185))
  - PR#185 has already been merged into PlasTeX (but the implementation might be helpful to other back-port problems)
  - changes `plasTeX/__init__.py`
  - see `handPatches/plasTeX/__init__.py.diff` (which passes local PyTests)
  - this particular change is a fairly radical and has not been implemented
  - ***This decision requires Stacks-testing***

### Commits from this repo's analysis

- Implement romannumeral [4dbe830]()
  - changes `plasTeX/Base/TeX/Macros.py`
  - changes `plasTeX/Base/TeX/__init__.py`
  - `filesChanged/plasTeX/Base/TeX/Macros.py/2017_08_19-19_11_59-4dbe830.diff` (passes local PyTests)
  - `handPatches/plasTeX/Base/TeX/__init__.py.diff` (passes local PyTests)

## Old

This directory contains a sub-directory for each potential backport. Each
backport is named using the backport's commit date (in `YYYY-MM-DD` order)
together with the backport's corresponding commit short code.

Each of these backport sub-directories contains a brief description of the
impact of the backport together with both the original and
plastex-code-base commit differences.
