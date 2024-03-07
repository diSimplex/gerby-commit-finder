# test_index_grouping

## Test failure

```
_____________________________ test_index_grouping ______________________________

    def test_index_grouping():
        tex = TeX()
        tex.input(r'''
        \documentclass{article}
        \usepackage{makeidx}
        \makeindex
        \begin{document}
        A
        \index{Abc!a}\index{Bbcd}\index{Abc!b}\index{Cdd}
        \printindex
        \end{document}
        ''')
        output = tex.parse()
>       assert len(output.userdata['links']['index']) == 3
E       assert 0 == 3
E        +  where 0 = len(<printindex element at 0x140143039312912>)

unittests/index.py:31: AssertionError
```
