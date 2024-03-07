# Changes to the handling of Accents

## Effected tests

- attempt-1-20240306 (4/28 failures) :
  - [test_accent.md](../attempt-1-20240306/test_accent.md)
  - [test_combining.md](../attempt-1-20240306/test_combining.md)
  - [test_empty_accent.md](../attempt-1-20240306/test_empty_accent.md)
  - [test_middle_combining.md](../attempt-1-20240306/test_middle_combining.md)


## Problem

This is the result of changes to the handling of Accents caused by the
commit `2017_08_20-10_39_10-4176b6a`

```
diff --git a/plasTeX/Base/LaTeX/Accents.py b/plasTeX/Base/LaTeX/Accents.py
index b7f6f8c..35c44ca 100644
--- a/plasTeX/Base/LaTeX/Accents.py
+++ b/plasTeX/Base/LaTeX/Accents.py
@@ -21,12 +21,16 @@ class Accent(Command):
     def str(self):
         return type(self).chars.get(self.textContent.strip(), None)

-    @property
+    @property
+    def source(self):
+        return type(self).chars.get(self.textContent.strip(), None)
+
+    @property
     def textContent(self):
         """
-        We need a customized textContent that doesn't look up
+        We need a customized textContent that doesn't look up
         textContent recursively.
-
+
         """
         output = []
         for item in self:

```

The commit is intended to solve [Unhandled mathematics in 0BM6: handle
accents inside text mode inside math · Issue #4 ·
gerby-project/plastex](https://github.com/gerby-project/plastex/issues/4)

## Solution

For [test_accent.md](../attempt-1-20240306/test_accent.md) unittest
failure the best solution is to simply alter the unittest itself.

For the other unittest failures, we need to update the code:

```
    @property
    def source(self):
        return type(self).chars.get(self.textContent.strip(), None)
```

to be:

```
    @property
    def source(self):
        checkedSource = type(self).chars.get(self.textContent.strip(), None)
        if checkedSource :
          return checkedSource
        return super().source()
```

This change checks to see if the new code returns a valid accent, and if
not then simply defaults to the super class's old behaviour.
