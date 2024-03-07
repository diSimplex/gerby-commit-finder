# test_combining

## Test failure

```
________________________________ test_combining ________________________________

    def test_combining():
        input_data = r'\c{o}'
        tex = TeX()
        tex.input(input_data)
        node = tex.parse()[0]
>       assert node.source == input_data

unittests/accents.py:17:
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

self = <c element at 0x140143042351120>

    @property
    def source(self):
        checkedSource = type(self).chars.get(self.textContent.strip(), None)
        if checkedSource :
            return checkedSource
>       return super().source()
E       TypeError: 'str' object is not callable

.tox/py311/lib/python3.11/site-packages/plasTeX/Base/LaTeX/Accents.py:37: TypeError
```
