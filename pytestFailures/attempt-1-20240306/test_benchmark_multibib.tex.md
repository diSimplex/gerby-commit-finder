# test_benchmark[multibib.tex]

## Test failure

```
_________________________ test_benchmark[multibib.tex] _________________________

src = PosixPath('/home/stg/tmp/gerby-plastex/b/unittests/Packages/sources/multibib.tex')
tmpdir = PosixPath('/tmp/pytest-of-stg/pytest-2/test_benchmark_multibib_tex_0')

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
E             @@ -83,7 +83,7 @@
E
E              </li>
E              </ol>
E              <div class="bibliography">
E             -<h1>Bibliography</h1>
E             +<h1></h1>
E              <dl class="bibliography">
E                <dt><a name="hagen:metafun">1</a></dt>
E                <dd><p>Hans Hagen. Metafun. Preliminary Version October 27, 2000, October 2000. <span class="ttfamily">http://www.pragma-ade.com</span>. </p>
E             plasTeX log:
E              (loading package /home/stg/tmp/gerby-
E                plastex/b/.tox/py311/lib/python3.11/site-
E                packages/plasTeX/Packages/article.py
E             ERROR: Loading package "article" raised exception EndInput :
E              (loading package /home/stg/tmp/gerby-
E                plastex/b/.tox/py311/lib/python3.11/site-
E                packages/plasTeX/Packages/natbib.py
E              )...
E             WARNING: unrecognized command/environment: chaptername
E             WARNING: unrecognized command/environment: bibname
E             .........
E             INFO: Directing output files to directory: /tmp/pytest-of-
E                stg/pytest-2/test_benchmark_multibib_tex_0.
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
E              [ multibib.html ]plasTeX version 3.1
E
E
E           assert not '--- benchmark\n\n+++ output\n\n@@ -83,7 +83,7 @@\n\n </li>\n </ol>\n <div class="bibliography">\n-<h1>Bibliography</h1>\n+<h1></h1>\n <dl class="bibliography">\n   <dt><a name="hagen:metafun">1</a></dt>\n   <dd><p>Hans Hagen. Metafun. Preliminary Version October 27, 2000, October 2000. <span class="ttfamily">http://www.pragma-ade.com</span>. </p>'

unittests/FunctionalTests.py:130: AssertionError

```