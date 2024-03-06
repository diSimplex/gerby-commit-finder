# NewCommands.testNewEnvironment

## Test failure

```
_______________________ NewCommands.testNewEnvironment ________________________

self = <plasTeX.Tokenizer.Tokenizer object at 0x7f1c8e99a250>

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

self = <NewCommands.NewCommands testMethod=testNewEnvironment>

    def testNewEnvironment(self):
        s = TeX()
        s.input(r'\newenvironment{myenv}{\begin{description}}{\end{description}}before:\begin{myenv}hi\end{myenv}:after')
        for key, value in self.macros.items():
            s.ownerDocument.context[key] = value
>       output = [x for x in s]

unittests/NewCommands.py:143:
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _
unittests/NewCommands.py:143: in <listcomp>
    output = [x for x in s]
.tox/py311/lib/python3.11/site-packages/plasTeX/TeX.py:322: in __iter__
    tokens = obj.invoke(self)
.tox/py311/lib/python3.11/site-packages/plasTeX/Base/LaTeX/Definitions.py:51: in invoke
    self.ownerDocument.context.newenvironment(*args, **kwargs)
.tox/py311/lib/python3.11/site-packages/plasTeX/Context.py:1118: in newenvironment
    def_before = list(Tokenizer(def_before, self))
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

self = <plasTeX.Tokenizer.Tokenizer object at 0x7f1c8e99a250>

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
ERROR: Error while expanding "newenvironment" in <string> on line 1

```