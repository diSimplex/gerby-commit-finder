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
E       AssertionError: assert None == '\\t{op}'
E        +  where None = <t element at 0x139760576055376>.source

unittests/accents.py:24: AssertionError

```