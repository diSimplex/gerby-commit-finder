# NodeTest.testNormalize

## Potential solutions

- [Lack of a context attribute on
  ownerDocuments](../solutions/ownerDocument.context.md)

## Test failure

```
____________________________ NodeTest.testNormalize ____________________________

self = <DOM.Node.NodeTest testMethod=testNormalize>

    def testNormalize(self):
        doc = Document()
        node = doc.createElement('node')
        one = doc.createElement('one')
        two = doc.createTextNode('two')
        three = doc.createTextNode('three')
        four = doc.createTextNode('four')
        node.extend([one,two,three,four])
>       node.normalize()

unittests/DOM/Node.py:465:
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

self = <node element at 0x139760624941392>, charsubs = []

    def normalize(self, charsubs=None):
        """
        Combine consecutive text nodes and remove comments

        Keyword Arguments:
        charsubs -- a list of two-element tuples that contain string
            replacements.  The first element in each tuple is the source
            string.  The second element is the string to convert the
            source to.

        """
        charsubs = charsubs or []
>       if not getattr(self, "doCharSubs", True) or self.ownerDocument.context.isMathMode:
E       AttributeError: 'Document' object has no attribute 'context'

.tox/py311/lib/python3.11/site-packages/plasTeX/DOM/__init__.py:1074: AttributeError
```
