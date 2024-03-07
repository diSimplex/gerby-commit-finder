# Accents str not callable

## Effected tests

- attempt-2-20240307:
  - [test_combining.md](../attempt-2-20240307/test_combining.md)
  - [test_empty_accent.md](../attempt-2-20240307/test_empty_accent.md)
  - [test_middle_combining.md](../attempt-2-20240307/test_middle_combining.md)

## Problem

The code `@property` above the sub-class's redefinition of `source(self)`
means that `super().source()` attempts to call a string which (surprise
surprise) is not what we wanted.

## Solution

Instead of the solution outlined in
[pytestFailures/solutions/changesToAccentHandling.md](changesToAccentHandling.md)
we should use:

```
    @property
    def source(self):
        textSrc = self.textContent.strip()
        theChars = type(self).chars
        if textSrc in theChars :
          return theChars[textSrc]
        return super().source
```
