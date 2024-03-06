# test_benchmark[babel.tex]

## Test failure

```
__________________________ test_benchmark[babel.tex] ___________________________

src = PosixPath('/home/stg/tmp/gerby-plastex/b/unittests/Packages/sources/babel.tex')
tmpdir = PosixPath('/tmp/pytest-of-stg/pytest-2/test_benchmark_babel_tex_0')

    @pytest.mark.parametrize('src', files,
                             ids=lambda p: p.name)
    def test_benchmark(src, tmpdir):
        tmpdir = Path(str(tmpdir)) # For old python
        root = src.parent.parent

        # Create temp dir and files
        texfile = tmpdir/src.name
        shutil.copyfile(str(src), str(texfile))
        outdir = str(tmpdir)

        # Run preprocessing commands
        with src.open() as fh:
            for line in fh:
                if line.startswith('%*'):
                    command = line[2:].strip()
                    p = Process(cwd=outdir, *command.split())
                    if p.returncode:
                        raise OSError('Preprocessing command exited abnormally with return code %s: %s' % (command, p.log))
                elif line.startswith('%#'):
                    filename = line[2:].strip()
                    shutil.copyfile(str(root/'extras'/filename),
                                    str(tmpdir/filename))
                elif line.startswith('%python '):
                    # skip this test on old python
                    version = tuple(int(n) for n in line[8:].strip().split('.'))
                    if sys.version_info < version:
                        pytest.skip('This python is too old')
                elif line.startswith('%'):
                    continue
                elif not line.strip():
                    continue
                else:
                    break

        # Run plastex
        outfile = (tmpdir/src.name).with_suffix('.html')
        plastex = which('plastex') or 'plastex'
        python = sys.executable
        p = Process(python, plastex,'--renderer=HTML5',
                    '--split-level=0','--no-theme-extras',
                    '--dir=%s' % outdir,'--theme=minimal',
                    '--filename=%s' % outfile.name, src.name,
                    cwd=outdir)
        if p.returncode:
            shutil.rmtree(outdir, ignore_errors=True)
            raise OSError('plastex failed with code %s: %s' % (p.returncode, p.log))

        benchfile = root/'benchmarks'/outfile.name
        if benchfile.exists():
            bench = benchfile.read_text().split('\n')
            output = outfile.read_text().split('\n')
        else:
            (root/'new').mkdir(parents=True, exist_ok=True)
            shutil.copyfile(str(outfile), str(root/'new'/outfile.name))
            shutil.rmtree(outdir, ignore_errors=True)
            raise OSError('No benchmark file: %s' % benchfile)

        # Compare files
        diff = '\n'.join(difflib.unified_diff(bench, output,
            fromfile='benchmark', tofile='output')).strip()

        if diff:
            (root/'new').mkdir(parents=True, exist_ok=True)
            shutil.copyfile(str(outfile), str(root/'new'/outfile.name))
            shutil.rmtree(outdir, ignore_errors=True)
>           assert not(diff), 'Differences were found: {}\nplasTeX log:\n{}'.format(diff,  p.log)
E           AssertionError: Differences were found: --- benchmark
E
E             +++ output
E
E             @@ -15,9 +15,9 @@
E
E              </head>
E
E              <body>
E             -<p>american figure name: Figure</p>
E             -<p>spanish figure name: Figura </p>
E             -<p> german figure name: Abbildung </p>
E             +<p>american figure name: </p>
E             +<p>spanishspanish figure name:  </p>
E             +<p>german german figure name:  </p>
E
E              </body>
E              </html>
E             plasTeX log:
E              (loading package /home/stg/tmp/gerby-
E                plastex/b/.tox/py311/lib/python3.11/site-
E                packages/plasTeX/Packages/article.py
E             ERROR: Loading package "article" raised exception EndInput :
E             ..
E              (loading package /home/stg/tmp/gerby-
E                plastex/b/.tox/py311/lib/python3.11/site-packages/plasTeX/Packages/babel.py
E             ERROR: Loading package "babel" raised exception EndInput :
E             ...
E             WARNING: unrecognized command/environment: figurename
E             .
E             WARNING: unrecognized command/environment: foreignlanguage
E             .
E             WARNING: unrecognized command/environment: otherlanguage
E             .
E             INFO: Directing output files to directory: /tmp/pytest-of-
E                stg/pytest-2/test_benchmark_babel_tex_0.
E             INFO: Importing templates from /home/stg/tmp/gerby-
E                plastex/b/.tox/py311/lib/python3.11/site-
E                packages/plasTeX/Renderers/PageTemplate
E             INFO: Importing templates from /home/stg/tmp/gerby-
E                plastex/b/.tox/py311/lib/python3.11/site-packages/plasTeX/Renderers/HTML5
E             INFO: Using theme /home/stg/tmp/gerby-
E                plastex/b/.tox/py311/lib/python3.11/site-
E                packages/plasTeX/Renderers/HTML5/Themes/minimal
E             INFO: Using the imager "gspdfpng".
E             INFO: Using the vector imager "pdf2svg".
E              [ babel.html
E             WARNING: Using default renderer for figurename
E              ]plasTeX version 3.1
E
E
E           assert not '--- benchmark\n\n+++ output\n\n@@ -15,9 +15,9 @@\n\n </head>\n \n <body>\n-<p>american figure name: Figure</p>\n-<p>spanish figure name: Figura </p>\n-<p> german figure name: Abbildung </p>\n+<p>american figure name: </p>\n+<p>spanishspanish figure name:  </p>\n+<p>german german figure name:  </p>\n \n </body>\n </html>'

unittests/FunctionalTests.py:130: AssertionError

```