# DocumentTest.testNormalizeDocument

## Potential solutions

- [Lack of a context attribute on
  ownerDocuments](../solutions/ownerDocument.context.md)

## Test failure

```
______________________ DocumentTest.testNormalizeDocument ______________________

self = <DOM.Document.DocumentTest testMethod=testNormalizeDocument>

    def testNormalizeDocument(self):
        doc = Document()
        one = doc.createElement('one')
        two = doc.createElement('two')
        text1 = doc.createTextNode('text1')
        text2 = doc.createTextNode('text2')
        text3 = doc.createTextNode('text3')

        doc.append(one)
        one.append(text1)
        one.append(two)
        two.extend([text2, text3])

>       doc.normalizeDocument()

unittests/DOM/Document.py:96:
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

self = <plasTeX.DOM.Document object at 0x7f1c8e152410>, charsubs = []

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

