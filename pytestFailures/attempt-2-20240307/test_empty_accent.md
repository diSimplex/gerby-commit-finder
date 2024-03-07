# test_empty_accent

## Test failure

```
______________________________ test_empty_accent _______________________________

    def test_empty_accent():
        input_data = r'\~{}'
        tex = TeX()
        tex.input(input_data)
        node = tex.parse()[0]
>       assert node.source == input_data

unittests/accents.py:33:
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

self = <~ element at 0x140143044270352>

    @property
    def source(self):
        checkedSource = type(self).chars.get(self.textContent.strip(), None)
        if checkedSource :
            return checkedSource
>       return super().source()
E       TypeError: 'str' object is not callable

.tox/py311/lib/python3.11/site-packages/plasTeX/Base/LaTeX/Accents.py:37: TypeError
```
