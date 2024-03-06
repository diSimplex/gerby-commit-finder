# EndInput related failures

## Effected tests

- attempt-1-20240306 (22/28 failures) :
  - [ContextGenerated.testNewcommand.md](../attempt-1-20240306/ContextGenerated.testNewcommand.md)
  - [NC.testNewenvironment.md](../attempt-1-20240306/NC.testNewenvironment.md)
  - [NewCommands.testNewEnvironment.md](../attempt-1-20240306/NewCommands.testNewEnvironment.md)
  - [NewCommands.testSimpleNewEnvironmentWithOptional.md](../attempt-1-20240306/NewCommands.testSimpleNewEnvironmentWithOptional.md)
  - [NewCommands.testSimpleNewEnvironmentWithArgs.md](../attempt-1-20240306/NewCommands.testSimpleNewEnvironmentWithArgs.md)

  - [NewCommands.testSimpleNewEnvironment.md](../attempt-1-20240306/NewCommands.testSimpleNewEnvironment.md)
  - [Parameters.testNumberSection.md](../attempt-1-20240306/Parameters.testNumberSection.md)
  - [Parameters.testRomanNumeralSection.md](../attempt-1-20240306/Parameters.testRomanNumeralSection.md)
  - [test_amsthm.md](../attempt-1-20240306/test_amsthm.md)
  - [test_benchmark_align.tex.md](../attempt-1-20240306/test_benchmark_align.tex.md)

  - [test_benchmark_babel.tex.md](../attempt-1-20240306/test_benchmark_babel.tex.md)
  - [test_benchmark_bib.tex.md](../attempt-1-20240306/test_benchmark_bib.tex.md)
  - [test_benchmark_footnotes.tex.md](../attempt-1-20240306/test_benchmark_footnotes.tex.md)
  - [test_benchmark_multibib.tex.md](../attempt-1-20240306/test_benchmark_multibib.tex.md)
  - [test_benchmark_natbib.tex.md](../attempt-1-20240306/test_benchmark_natbib.tex.md)

  - [test_index_entries_are_there.md](../attempt-1-20240306/test_index_entries_are_there.md)
  - [test_index_grouping.md](../attempt-1-20240306/test_index_grouping.md)
  - [test_index_sorting.md](../attempt-1-20240306/test_index_sorting.md)
  - [test_load_class_article.md](../attempt-1-20240306/test_load_class_article.md)
  - [test_load_class_book.md](../attempt-1-20240306/test_load_class_book.md)

  - [test_load_class_report.md](../attempt-1-20240306/test_load_class_report.md)
  - [test_numberwithin.md](../attempt-1-20240306/test_numberwithin.md)


## Problem

The two commit `2019_10_19-10_45_23-650be75` changed two files:

- `plasTeX/Tokenizer.py`

```
diff --git a/plasTeX/Tokenizer.py b/plasTeX/Tokenizer.py
index 98715eb..d9d5c85 100644
--- a/plasTeX/Tokenizer.py
+++ b/plasTeX/Tokenizer.py
@@ -30,6 +30,9 @@ DEFAULT_CATEGORIES = [
 VERBATIM_CATEGORIES = [''] * 16
 VERBATIM_CATEGORIES[11] = encoding.stringletters()

+class EndInput(Exception):
+    pass
+
 class Token(Text):
     """ Base class for all TeX tokens """

@@ -383,7 +386,10 @@ class Tokenizer(object):
                 yield mybuffer.pop(0)

             # Get the next character
-            token = next(charIter)
+            try:
+                token = next(charIter)
+            except StopIteration:
+                raise EndInput

             if token.nodeType == ELEMENT_NODE:
                 raise ValueError('Expanded tokens should never make it here')
```

- `plasTeX/TeX.py`

```
diff --git a/plasTeX/TeX.py b/plasTeX/TeX.py
index 3ad42ee..2f4fab7 100644
--- a/plasTeX/TeX.py
+++ b/plasTeX/TeX.py
@@ -17,7 +17,7 @@ Example:
 """
 from io import IOBase
 import string, os, traceback, sys, plasTeX, subprocess, types
-from plasTeX.Tokenizer import Tokenizer, Token, EscapeSequence, Other
+from plasTeX.Tokenizer import Tokenizer, Token, EscapeSequence, Other, EndInput
 from plasTeX import TeXDocument
 from plasTeX.Base.TeX.Primitives import MathShift
 from plasTeX import ParameterCommand, Macro
@@ -270,7 +270,7 @@ class TeX(object):
                     t.parentNode = None
                     yield t

-            except StopIteration:
+            except (EndInput, StopIteration):
                 endInput()

             # This really shouldn't happen, but just in case...
@@ -319,7 +319,10 @@ class TeX(object):

         while 1:
             # Get the next token
-            token = next()
+            try:
+                token = next()
+            except StopIteration:
+                return

             # Token is null, ignore it
             if token is None:
```

Unfortunately the `EndInput` exception is not caught (and handled) in
enough places through out the PlasTeX code to protect against this change.

## Solution

Since this commit on 2019/Oct/19, no further uses of the `EndInput` are
made anywhere else in the Gerby-PlasTeX code itself.

```
external/gerby-plastex$ ag EndInput
plasTeX/TeX.py
20:from plasTeX.Tokenizer import Tokenizer, Token, EscapeSequence, Other, EndInput
273:            except (EndInput, StopIteration):

plasTeX/Tokenizer.py
33:class EndInput(Exception):
392:                raise EndInput

```

Since the PlasTeX code base (correctly) handles it use of the
`StopIteration` exception, and since the Gerby-PlasTeX code base makes no
independent use of the `EndInput` exception, I propose we ignore commit
`2019_10_19-10_45_23-650be75`.



