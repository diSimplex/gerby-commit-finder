# test_amsthm

## Test failure

```
_________________________________ test_amsthm __________________________________

self = <plasTeX.Tokenizer.Tokenizer object at 0x7f1c8bd173d0>

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

tmpdir = local('/tmp/pytest-of-stg/pytest-2/test_amsthm0')

    def test_amsthm(tmpdir):
        root = Path(__file__).parent
        config = defaultConfig()
        addConfig(config)
        config['files']['split-level'] = -100
        tex = TeX(TeXDocument(config=config))
        tex.input((root/'source.tex').read_text())
>       doc = tex.parse()

unittests/amsthm/amsthm.py:17:
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _
.tox/py311/lib/python3.11/site-packages/plasTeX/TeX.py:413: in parse
    for item in tokens:
.tox/py311/lib/python3.11/site-packages/plasTeX/TeX.py:44: in __next__
    return self._next()
.tox/py311/lib/python3.11/site-packages/plasTeX/TeX.py:322: in __iter__
    tokens = obj.invoke(self)
.tox/py311/lib/python3.11/site-packages/plasTeX/Packages/amsthm.py:183: in invoke
    self.ownerDocument.context.newcommand("the"+name, 0,
.tox/py311/lib/python3.11/site-packages/plasTeX/Context.py:1077: in newcommand
    definition = list(Tokenizer(definition, self))
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

self = <plasTeX.Tokenizer.Tokenizer object at 0x7f1c8bd173d0>

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
----------------------------- Captured stderr call -----------------------------
ERROR: Loading package "article" raised exception EndInput :
WARNING: unrecognized command/environment: thmname
WARNING: unrecognized command/environment: thmnumber
WARNING: unrecognized command/environment: thmnote
ERROR: Error while expanding "newtheorem" in <string> on line 18
ERROR: An error occurred while building the document object in <string> on
   line 18

```