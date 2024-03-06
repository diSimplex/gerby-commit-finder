# test_numberwithin

## Test failure

```
______________________________ test_numberwithin _______________________________

    def test_numberwithin():
        s = TeX()
        s.input(DOC_numberwithin)
        output = s.parse()
        equations =output.getElementsByTagName('equation')
>       assert equations[0].ref.textContent == '1.1'
E       AssertionError: assert '0.1.1' == '1.1'
E
E         - 1.1
E         + 0.1.1
E         ? ++

unittests/Packages/Amsmath.py:28: AssertionError
----------------------------- Captured stderr call -----------------------------
ERROR: Loading package "article" raised exception EndInput :
WARNING: unrecognized command/environment: sectionname
WARNING: unrecognized command/environment: subsectionname

```