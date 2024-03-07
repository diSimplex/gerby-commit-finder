# test_index_sorting

## Test failure

```
______________________________ test_index_sorting ______________________________

    def test_index_sorting():
        tex = TeX()
        tex.input(r'''
        \documentclass{article}
        \usepackage{makeidx}
        \makeindex
        \begin{document}
        A
        \index{0@$\to$}\index{0@$\in$}
        \printindex
        \end{document}
        ''')
        output = tex.parse()
>       assert len(output.userdata['links']['index']) == 2
E       assert 0 == 2
E        +  where 0 = len(<printindex element at 0x140143041144144>)

unittests/index.py:46: AssertionError
```
