# test_benchmark[footnotes.tex]

## Test failure

```
________________________ test_benchmark[footnotes.tex] _________________________

src = PosixPath('/home/stg/tmp/gerby-plastex/b/unittests/sources/footnotes.tex')
tmpdir = PosixPath('/tmp/pytest-of-stg/pytest-2/test_benchmark_footnotes_tex_0')

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
E             @@ -15,21 +15,21 @@
E
E              </head>
E
E              <body>
E             -<h1 id="a0000000002">1 First Section</h1>
E             +<h1 id="a0000000002">0.1 First Section</h1>
E              <p>Vivamus congue leo sed nulla. In hac habitasse platea dictumst. Nulla congue suscipit ante. Quisque in risus. Nullam dignissim arcu ac libero. Duis adipiscing lacinia neque. Aenean velit justo, sagittis et, pharetra sed<a class="footnote" href="#footnotes">
E                <sup class="footnotemark">1</sup>
E              </a>, semper ut, metus. Vestibulum sit amet dolor vitae lacus cursus auctor. Suspendisse<a class="footnote" href="#footnotes">
E                <sup class="footnotemark">2</sup>
E              </a>Yet another footnote in metus a ipsum bibendum malesuada. Aenean ac quam vel lectus aliquam facilisis. Integer dolor. Ut nunc. Ut interdum lacus eu pede. </p>
E
E             -<h2 id="a0000000003">1.1 First Subsection</h2>
E             +<h2 id="a0000000003">0.1.1 First Subsection</h2>
E              <p>Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Sed euismod velit. Aliquam quis enim ut erat semper eleifend. Mauris dignissim, elit id rhoncus porta, justo nulla vestibulum justo, varius euismod nisl massa ut ligula<a class="footnote" href="#a0000000004">
E                <sup class="footnotemark">3</sup>
E              </a>. Quisque tincidunt iaculis sapien. Aliquam urna mi, venenatis at, lobortis sed, cursus in, leo. Sed eget massa. Aenean tempor. Phasellus consectetuer velit id dolor. Vivamus in lorem. Integer sodales porttitor risus. Sed nunc tortor, tristique et, ultricies id, vestibulum nec, mi.<a class="footnote" href="#a0000000005">
E                <sup class="footnotemark">4</sup>
E              </a> </p>
E              <p>Vivamus congue leo sed nulla. In hac habitasse platea dictumst. Nulla congue suscipit ante. Quisque in risus. Nullam dignissim arcu ac libero. Duis adipiscing lacinia neque. Aenean velit justo, sagittis et, pharetra sed, semper ut, metus. Vestibulum sit amet dolor vitae lacus cursus auctor. Suspendisse in metus a ipsum bibendum malesuada. Aenean ac quam vel lectus aliquam facilisis. Integer dolor. Ut nunc. Ut interdum lacus eu pede. </p>
E             -<h1 id="a0000000006">2 Second Section</h1>
E             +<h1 id="a0000000006">0.2 Second Section</h1>
E              <p>Etiam gravida, lectus nec lacinia imperdiet, dolor enim aliquet augue, id scelerisque lorem tortor ac massa. Ut eu odio ut lectus varius condimentum. Suspendisse molestie. Phasellus eget felis. Donec eget velit sed quam scelerisque faucibus. Suspendisse potenti. Vivamus sed dolor ac neque molestie ornare. Suspendisse ultricies, justo vitae varius auctor, ante arcu pulvinar augue<a class="footnote" href="#a0000000007">
E                <sup class="footnotemark">5</sup>
E              </a>, in suscipit augue magna tempus massa. Ut semper. Praesent felis ipsum, porttitor vitae, ullamcorper sed, pharetra vel, felis. Praesent nec tortor ullamcorper tellus iaculis scelerisque. In dapibus massa nec mauris. Nulla facilisi. Donec vel odio quis eros nonummy viverra. Maecenas mollis tortor eget ante. </p>
E             plasTeX log:
E              (loading package /home/stg/tmp/gerby-
E                plastex/b/.tox/py311/lib/python3.11/site-
E                packages/plasTeX/Packages/article.py
E             ERROR: Loading package "article" raised exception EndInput :
E             ...
E             WARNING: unrecognized command/environment: sectionname
E             ......
E             WARNING: unrecognized command/environment: subsectionname
E             ..........
E             INFO: Directing output files to directory: /tmp/pytest-of-
E                stg/pytest-2/test_benchmark_footnotes_tex_0.
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
E              [ footnotes.html ]plasTeX version 3.1
E
E
E           assert not '--- benchmark\n\n+++ output\n\n@@ -15,21 +15,21 @@\n\n </head>\n \n <body>\n-<h1 id="a0000000002">1 First Section</h1>\n+<h1 id="a0000000002">0.1 First Section</h1>\n <p>Vivamus congue leo sed nulla. In hac habitasse platea dictumst. Nulla congue suscipit ante. Quisque in risus. Nullam dignissim arcu ac libero. Duis adipiscing lacinia neque. Aenean velit justo, sagittis et, pharetra sed<a class="footnote" href="#footnotes">\n   <sup class="footnotemark">1</sup>\n </a>, semper ut, metus. Vestibulum sit amet dolor vitae lacus cursus auctor. Suspendisse<a class="footnote" href="#footnotes">\n   <sup class="footnotemark">2</sup>\n </a>Yet another footnote in metus a ipsum bibendum malesuada. Aenean ac quam vel lectus aliquam facilisis. Integer dolor. Ut nunc. Ut interdum lacus eu pede. </p>\n \n-<h2 id="a0000000003">1.1 First Subsection</h2>\n+<h2 id="a0000000003">0.1.1 First Subsection</h2>\n <p>Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Sed euismod velit. Aliquam quis enim ut erat semper eleifend. Mauris dignissim, elit id rhoncus porta, justo nulla vestibulum justo, varius euismod nisl massa ut ligula<a class="footnote" href="#a0000000004">\n   <sup class="footnote...nissim arcu ac libero. Duis adipiscing lacinia neque. Aenean velit justo, sagittis et, pharetra sed, semper ut, metus. Vestibulum sit amet dolor vitae lacus cursus auctor. Suspendisse in metus a ipsum bibendum malesuada. Aenean ac quam vel lectus aliquam facilisis. Integer dolor. Ut nunc. Ut interdum lacus eu pede. </p>\n-<h1 id="a0000000006">2 Second Section</h1>\n+<h1 id="a0000000006">0.2 Second Section</h1>\n <p>Etiam gravida, lectus nec lacinia imperdiet, dolor enim aliquet augue, id scelerisque lorem tortor ac massa. Ut eu odio ut lectus varius condimentum. Suspendisse molestie. Phasellus eget felis. Donec eget velit sed quam scelerisque faucibus. Suspendisse potenti. Vivamus sed dolor ac neque molestie ornare. Suspendisse ultricies, justo vitae varius auctor, ante arcu pulvinar augue<a class="footnote" href="#a0000000007">\n   <sup class="footnotemark">5</sup>\n </a>, in suscipit augue magna tempus massa. Ut semper. Praesent felis ipsum, porttitor vitae, ullamcorper sed, pharetra vel, felis. Praesent nec tortor ullamcorper tellus iaculis scelerisque. In dapibus massa nec mauris. Nulla facilisi. Donec vel odio quis eros nonummy viverra. Maecenas mollis tortor eget ante. </p>'

unittests/FunctionalTests.py:130: AssertionError

```