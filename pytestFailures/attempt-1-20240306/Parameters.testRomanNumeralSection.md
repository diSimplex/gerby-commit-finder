# Parameters.testRomanNumeralSection

## Test failure

```
______________________ Parameters.testRomanNumeralSection ______________________

self = <Numbers.Parameters testMethod=testRomanNumeralSection>

        def testRomanNumeralSection(self):
            t = TeX()
            t.input(r'''
    \documentclass{article}
    \begin{document}
    \section{}
    \section{}
    \romannumeral\thesection
    \end{document}
    ''')
>           assert t.parse().textContent.strip() == 'ii'

unittests/Numbers.py:254:
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _
.tox/py311/lib/python3.11/site-packages/plasTeX/TeX.py:416: in parse
    item.digest(tokens)
.tox/py311/lib/python3.11/site-packages/plasTeX/__init__.py:944: in digest
    item.digest(tokens)
.tox/py311/lib/python3.11/site-packages/plasTeX/Base/LaTeX/Sectioning.py:277: in digest
    for item in tokens:
.tox/py311/lib/python3.11/site-packages/plasTeX/TeX.py:44: in __next__
    return self._next()
.tox/py311/lib/python3.11/site-packages/plasTeX/TeX.py:322: in __iter__
    tokens = obj.invoke(self)
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

self = <romannumeral element at 0x139760626825808>
tex = <plasTeX.TeX.TeX object at 0x7f1c90d42810>

    def invoke(self, tex):
        a = self.parse(tex)
        roman = ""
>       n, number = divmod(int(a['name']), 1000)
E       ValueError: invalid literal for int() with base 10: '0.2'

.tox/py311/lib/python3.11/site-packages/plasTeX/Base/TeX/Macros.py:13: ValueError
----------------------------- Captured stderr call -----------------------------
ERROR: Loading package "article" raised exception EndInput :
WARNING: unrecognized command/environment: sectionname
ERROR: Error while expanding "romannumeral" in <string> on line 6 (invalid
   literal for int() with base 10: '0.2')
ERROR: An error occurred while building the document object in <string> on
   line 6 (invalid literal for int() with base 10: '0.2')

```