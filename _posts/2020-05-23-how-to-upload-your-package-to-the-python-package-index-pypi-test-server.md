---
layout: post
title: How to upload your package to the Python Package Index (PyPI) test server
category: posts
---

Python packages are distributed via the Python Package Index ([PyPI]). The
[Python Packaging Guide] provides details for [uploading your project to PyPI].
However the packaging guide is missing instructions for uploading to the
[PyPI test server]. This is [recommended] as a trial run for uploading a new
version of your package, since you upload a given version of your project only
*once*. You can use the test server to e.g. verify your `README` renders
correctly.

Assuming you have finished all the work on the new release of your project,
written the [release notes], [increased the version number], tagged the release
and are ready to publish, follow theses steps:

1.  If you haven't yet, create accounts on [PyPI] and the [PyPI test server].
    Note that these accounts are entirely independent, however you may want to
    use the same username (but different passwords, of course).

1.  For security reasons it is strongly recommended to create an [API token]
    instead of using your username and password when uploading a package to
    [PyPI]. If you haven't done so, create an [API token] on both [PyPI][1] and
    the [PyPI test server][2].

    You can choose to restrict this token to only a single package, which you
    should definitely do if you use the [API token] e.g. in a CI/CD workflow.
    For your personal use I suggest you leave the token unrestricted, since
    [there is no good workflow for switching between multiple API tokens][3].

    Note that the [API token] will only be displayed once when you create it,
    so make sure you copy the token. If you forget to do that, revoke and
    create a new one.

1.  Create a `.pypirc` file in your home directory to store your API tokens
    for authentication when uploading, with the following content:

    ```
    [pypi]
    username = __token__
    password = pypi-AgEIcH...

    [testpypi]
    username = __token__
    password = pypi-AgENdG...
    ```

1.  If you haven't done so, install [twine]: `pip install --upgrade twine`.

1.  Create a [source distribution] and a [wheel] for your package:

    ```
    python setup.py sdist bdist_wheel
    ```

1.  Run `twine check` on your package files and ensure they pass:

    ```
    $ twine check dist/*
    Checking dist/dokuwikixmlrpc-2020.5.23-py2.py3-none-any.whl: PASSED
    Checking dist/dokuwikixmlrpc-2020.5.23.tar.gz: PASSED
    ```

    This command will report any problems rendering your `README`.

1.  Upload your packages to the [PyPI test server]:

    ```
    twine upload --repository testpypi dist/*
    ```

    You should not be prompted for a username or password, since those are
    configure in your `.pypirc`. When successful, this should print the URL of
    your package on the test server:

    ```
    Uploading distributions to https://test.pypi.org/legacy/
    Uploading dokuwikixmlrpc-2020.5.23-py2.py3-none-any.whl
    100%|████████████████████████████| 10.1k/10.1k [00:01<00:00, 5.23kB/s]
    Uploading dokuwikixmlrpc-2020.5.23.tar.gz
    100%|████████████████████████████| 9.63k/9.63k [00:01<00:00, 9.39kB/s]

    View at:
    https://test.pypi.org/project/dokuwikixmlrpc/2020.5.23/
    ```

    Verify everything is as you expect e.g. there are no rendering errors.

1.  Upload your packages to [PyPI], fo realz:

    ```
    twine upload dist/*
    ```

    You should not be prompted for a username or password, since those are
    configure in your `.pypirc`. When successful, you should see output similar
    to the above.

Congratulations! You've done it! You package is now available to the world 🎉

[PyPI]: https://pypi.org
[Python Packaging Guide]: https://packaging.python.org
[uploading your project to PyPI]: https://packaging.python.org/guides/distributing-packages-using-setuptools/#uploading-your-project-to-pypi
[PyPI test server]: https://test.pypi.org
[recommended]: https://packaging.python.org/guides/using-testpypi/
[release notes]: https://keepachangelog.com
[increased the version number]: https://pypi.org/project/bump2version/
[API token]: https://pypi.org/help/#apitoken
[1]: https://pypi.org/manage/account/token/
[2]: https://test.pypi.org/manage/account/token/
[3]: https://github.com/pypa/twine/issues/496
[twine]: https://twine.readthedocs.io
[source distribution]: https://packaging.python.org/glossary/#term-source-distribution-or-sdist
[wheel]: https://packaging.python.org/guides/distributing-packages-using-setuptools/#wheels
