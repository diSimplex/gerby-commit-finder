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
E       AssertionError: assert None == '\\c{o}'
E        +  where None = <c element at 0x139760576216784>.source

unittests/accents.py:16: AssertionError

```