# test_index_entries_are_there

## Test failure

```
_________________________ test_index_entries_are_there _________________________

    def test_index_entries_are_there():
        tex = TeX()
        tex.input(r'''
          \documentclass{article}
          \begin{document}
          \index{test}
          \index{test2}
          \printindex
          \end{document}
          ''')
        output = tex.parse()
>       assert len(output.userdata['index']) == 2
E       KeyError: 'index'

unittests/index.py:16: KeyError
----------------------------- Captured stderr call -----------------------------
ERROR: Loading package "article" raised exception EndInput :
WARNING: unrecognized command/environment: chaptername
WARNING: unrecognized command/environment: indexname

```