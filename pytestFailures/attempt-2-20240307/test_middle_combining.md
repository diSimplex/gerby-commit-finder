# test_middle_combining

## Test failure

```
____________________________ test_middle_combining _____________________________

    def test_middle_combining():
        input_data = r'\t{op}'
        tex = TeX()
        tex.input(input_data)
        node = tex.parse()[0]
>       assert node.source == input_data

unittests/accents.py:25:
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

self = <t element at 0x140143042335632>

    @property
    def source(self):
        checkedSource = type(self).chars.get(self.textContent.strip(), None)
        if checkedSource :
            return checkedSource
>       return super().source()
E       TypeError: 'str' object is not callable

.tox/py311/lib/python3.11/site-packages/plasTeX/Base/LaTeX/Accents.py:37: TypeError
```
