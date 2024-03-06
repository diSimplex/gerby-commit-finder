# Lack of a context attribute on ownerDocuments

## Effected tests

- attempt-1-20240306 (2/28 failures) :
  - [ContextGenerated.testNewcommand](../attempt-1-20240306/ContextGenerated.testNewcommand.md)
  - [DocumentTest.testNormalizeDocument](../attempt-1-20240306/DocumentTest.testNormalizeDocument.md)

## General Problem

The offending code is the condition
`self.ownerDocument.context.isMathMode` in the patch
`2018_02_20-23_19_31-3722774`:

```
diff --git a/plasTeX/DOM/__init__.py b/plasTeX/DOM/__init__.py
index a20f14f..6cf0549 100644
--- a/plasTeX/DOM/__init__.py
+++ b/plasTeX/DOM/__init__.py
@@ -1070,6 +1070,9 @@ class Node(object):

         """
         charsubs = charsubs or []
+        if not getattr(self, "doCharSubs", True) or self.ownerDocument.context.isMathMode:
+            charsubs = []
+
         if self.hasAttributes():
             for value in list(self.attributes.values()):
                 if isinstance(value, Node):
```

The intent of this code is to provide tools to turn "character
substitution" off at a very deep level in the DOM hierarchy, and in
particular whenever we are "in math mode".

At the moment PlasTeX provides a number of mechanisms to turn character
substitution off in, for example, Verbatim environments by using the
`NoCharSubEnvironment`.

Note that at one point the Gerby project proposed a `NoCharSubCommand`.
However there is no current code defining or using the `NoCharSubCommand`
class. This lack of a `NoCharSubCommand` might be why the above code is
needed.

The important point is, there will always be points in the DOM which might
not be enclosed inside a `NoCharSubEnvironment` node, but for which
character substitution *should* be turned off.

## PyTest Problem

For these PyTests, the real problem is that the "ownerDocument" is ONLY
created for `TeXDocuments` not "general" `Documents`.

This means there are three solutions:

1. Re-write this code to add a check for whether or not the
   `ownerDocument` has a `context` attribute.

2. Change all instances of `Document` in these tests to `TeXDocument`.

3. Promote the following code ('plasTeX/\_\_init\_\_.py') :

```
    817         if 'context' not in list(kwargs.keys()):
    818             from plasTeX import Context
    819             self.context = Context.Context(load=True)
    820         else:
    821             self.context = kwargs['context']
```
from the `TeXDocument` class to the `Document` superclass.
