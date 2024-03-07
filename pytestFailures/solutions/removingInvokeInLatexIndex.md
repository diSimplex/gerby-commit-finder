# Removing Invoke for LaTeX Index

## Effected tests

- attempt-2-20240307 (3/7 failures) :
  - [test_index_entries_are_there.md](../attempt-2-20240307/test_index_entries_are_there.md)
  - [test_index_grouping.md](../attempt-2-20240307/test_index_grouping.md)
  - [test_index_sorting.md](../attempt-2-20240307/test_index_sorting.md)

## Problem

Commit `2018_01_03-17_20_05-bf4a617` removed the invoke function for the
`index(Command)` class:

```
diff --git a/plasTeX/Base/LaTeX/Index.py b/plasTeX/Base/LaTeX/Index.py
index 7e779c7..5a02b5c 100644
--- a/plasTeX/Base/LaTeX/Index.py
+++ b/plasTeX/Base/LaTeX/Index.py
@@ -279,6 +279,7 @@ class index(Command):
     def textContent(self):
         return ''

+"""
     def invoke(self, tex):
         result = Command.invoke(self, tex)
         sortkey, key, format = [], [], []
@@ -355,6 +356,7 @@ class index(Command):
         userdata['index'].append(IndexEntry(key, self, sortkey, format, type))

         return result
+"""
```

## Solution

One potential solution is to ignore this commit.

The fundamental question is will ignoring this commit break the Gerby
plugin (and in particular the Stacks-test)?
