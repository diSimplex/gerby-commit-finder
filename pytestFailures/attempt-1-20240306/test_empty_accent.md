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
E       AssertionError: assert None == '\\~{}'
E        +  where None = <~ element at 0x139760583076816>.source

unittests/accents.py:32: AssertionError

```