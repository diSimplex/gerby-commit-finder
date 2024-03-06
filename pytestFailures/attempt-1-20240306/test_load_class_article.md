# test_load_class[article]

## Test failure

```
___________________________ test_load_class[article] ___________________________

self = <plasTeX.Tokenizer.Tokenizer object at 0x7f1c8b3c82d0>

    def __iter__(self):
        """
        Iterate over tokens in the input stream

        Returns:
        generator that iterates through tokens in the stream

        """
        # Cache variables to prevent globol lookups during generator
        global Space, EscapeSequence
        Space = Space
        EscapeSequence = EscapeSequence
        tokenClasses = self.tokenClasses
        mybuffer = self._tokBuffer
        charIter = self.iterchars()
        context = self.context
        pushChar = self.pushChar
        STATE_N = self.STATE_N
        STATE_M = self.STATE_M
        STATE_S = self.STATE_S
        ELEMENT_NODE = Node.ELEMENT_NODE
        CC_LETTER = Token.CC_LETTER
        CC_OTHER = Token.CC_OTHER
        CC_SPACE = Token.CC_SPACE
        CC_EOL = Token.CC_EOL
        CC_ESCAPE = Token.CC_ESCAPE
        CC_EOL = Token.CC_EOL
        CC_COMMENT = Token.CC_COMMENT
        CC_ACTIVE = Token.CC_ACTIVE
        prev = None

        while 1:

            # Purge mybuffer first
            while mybuffer:
                yield mybuffer.pop(0)

            # Get the next character
            try:
>               (code, char) = next(charIter)
E               StopIteration

.tox/py311/lib/python3.11/site-packages/plasTeX/Tokenizer.py:376: StopIteration

During handling of the above exception, another exception occurred:

doc_class = 'article'

    @pytest.mark.parametrize("doc_class", ['article', 'report', 'book'])
    def test_load_class(doc_class):
        doc = TeXDocument()
        tex = TeX(doc)
>       doc.context.loadPackage(tex, doc_class)

unittests/load_doc_class.py:9:
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _
.tox/py311/lib/python3.11/site-packages/plasTeX/Context.py:542: in loadPackage
    if self.loadPythonPackage(tex.ownerDocument, file_name, options):
.tox/py311/lib/python3.11/site-packages/plasTeX/Context.py:494: in loadPythonPackage
    imported.ProcessOptions(options, document) # type: ignore
.tox/py311/lib/python3.11/site-packages/plasTeX/Packages/article.py:6: in ProcessOptions
    report.ProcessOptions(options, document)
.tox/py311/lib/python3.11/site-packages/plasTeX/Packages/report.py:5: in ProcessOptions
    book.ProcessOptions(options, document)
.tox/py311/lib/python3.11/site-packages/plasTeX/Packages/book.py:45: in ProcessOptions
    context.loadLanguage('american', document)
.tox/py311/lib/python3.11/site-packages/plasTeX/Context.py:326: in loadLanguage
    self.newcommand('languagename', definition=lang)
.tox/py311/lib/python3.11/site-packages/plasTeX/Context.py:1077: in newcommand
    definition = list(Tokenizer(definition, self))
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

self = <plasTeX.Tokenizer.Tokenizer object at 0x7f1c8b3c82d0>

    def __iter__(self):
        """
        Iterate over tokens in the input stream

        Returns:
        generator that iterates through tokens in the stream

        """
        # Cache variables to prevent globol lookups during generator
        global Space, EscapeSequence
        Space = Space
        EscapeSequence = EscapeSequence
        tokenClasses = self.tokenClasses
        mybuffer = self._tokBuffer
        charIter = self.iterchars()
        context = self.context
        pushChar = self.pushChar
        STATE_N = self.STATE_N
        STATE_M = self.STATE_M
        STATE_S = self.STATE_S
        ELEMENT_NODE = Node.ELEMENT_NODE
        CC_LETTER = Token.CC_LETTER
        CC_OTHER = Token.CC_OTHER
        CC_SPACE = Token.CC_SPACE
        CC_EOL = Token.CC_EOL
        CC_ESCAPE = Token.CC_ESCAPE
        CC_EOL = Token.CC_EOL
        CC_COMMENT = Token.CC_COMMENT
        CC_ACTIVE = Token.CC_ACTIVE
        prev = None

        while 1:

            # Purge mybuffer first
            while mybuffer:
                yield mybuffer.pop(0)

            # Get the next character
            try:
                (code, char) = next(charIter)
            except StopIteration:
>               raise EndInput
E               plasTeX.Tokenizer.EndInput

.tox/py311/lib/python3.11/site-packages/plasTeX/Tokenizer.py:378: EndInput

```