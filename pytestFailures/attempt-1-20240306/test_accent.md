# test_accent

## Test failure

```
_________________________________ test_accent __________________________________

    def test_accent():
        input_data = r'\"o'
        tex = TeX()
        tex.input(input_data)
        node = tex.parse()[0]
>       assert node.source == input_data
E       assert 'ö' == '\\"o'
E
E         - \"o
E         + ö

unittests/accents.py:8: AssertionError

```