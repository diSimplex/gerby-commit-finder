# Parameters.testNumberSection

## Test failure

```

_________________________ Parameters.testNumberSection _________________________

self = <Numbers.Parameters testMethod=testNumberSection>

        def testNumberSection(self):
            t = TeX()
            t.input(r'''
    \documentclass{article}
    \begin{document}
    \section{}
    \section{}
    \number\thesection
    \end{document}
    ''')
>           assert t.parse().textContent.strip() == '2'
E           AssertionError: assert '0' == '2'
E
E             - 2
E             + 0

unittests/Numbers.py:242: AssertionError
----------------------------- Captured stderr call -----------------------------
ERROR: Loading package "article" raised exception EndInput :
WARNING: unrecognized command/environment: sectionname

```